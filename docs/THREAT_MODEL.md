# THREAT MODEL — VERITAS v2

Ce document liste les attaques considérées contre VERITAS et la résistance offerte par chaque version.

---

## 1. Modèle d'adversaire

VERITAS est conçu pour résister à 4 profils d'adversaires :

| Profil | Capacités | Motivation |
|---|---|---|
| **Auteur honnête maladroit** | Calcule mais peut se tromper | Erreur arithmétique de bonne foi |
| **IA hallucinante** | Génère plausible sans vérifier | Producing the most likely-looking answer |
| **Fraudeur naïf** | Connait l'algorithme, pas la cryptanalyse | Avancement de carrière |
| **Fraudeur computationnel** | Peut bruteforce ~10⁹ tentatives | Publication majeure ou enjeu financier |

VERITAS **n'est pas conçu** pour résister à :
- Adversaire avec accès au code et capacité à le modifier
- État-nation avec ressources cryptographiques massives
- Compromission de la pré-registration (timestamp manipulé en amont)

---

## 2. Tableau des attaques

| # | Attaque | v1 | v2 | Notes |
|---:|---|:-:|:-:|---|
| 1 | Erreur honnête de calcul | ✅ | ✅ | PHARES rejette V incorrect |
| 2 | Hallucination IA (V faux, P faux assorti) | ✅ | ✅ | PHARES recompute V depuis E |
| 3 | Mensonge naïf sur M (somme opérandes) | ✅ | ✅ | MANE I rejette |
| 4 | Mensonge naïf sur T (nb opérateurs) | ✅ | ✅ | MANE II rejette |
| 5 | Bruteforce sur V mod 7 (~7 essais) | ❌ | 🟡 | v1 = 1/7 d'éviter, v2 = 1/7 mais aussi 1/11 et 1/13 |
| 6 | Bruteforce coordonné sur (M, T, P) | ❌ | ✅ | v1 = 1/343, v2 = 1/(7·11·13) |
| 7 | **Swap de formule** (garde la proposition) | ❌ | ✅ | v1 laisse passer ; v2 détecte par cross-link |
| 8 | Swap de proposition (garde la formule) | ✅ | ✅ | Les sceaux 1-6 vérifient l'intégrité de la prop |
| 9 | Faux raisonnement structuré (i0..i5 inventés) | ✅ | ✅ | Sceaux 1-6 chacun lit sa dimension |
| 10 | HARKing (intent ≠ pré-enregistré) | ✅ | ✅ | Sceau 1 rejette si i0 = 0 (la proposition exigée) |
| 11 | p-hacking (multiplications de tests) | ✅ | ✅ | Sceau 6 rejette si i5 ≥ 6 |
| 12 | Cherry-picking (cas-limites ignorés) | ✅ | ✅ | Sceau 4 rejette si i3 < 15 |
| 13 | Pas de réplication indépendante | ✅ | ✅ | Sceau 3 rejette si i2 = 0 |
| 14 | Data fabrication (Stapel-style) | ❌ | ❌ | Prévu en v3 (witness Δ dataset) |
| 15 | Backdating de la pré-registration | ❌ | ❌ | Prévu en v4 (Merkle tree timestamp) |
| 16 | Substitution post-publication | ❌ | 🟡 | v2 nécessite recalculer fhash + i6 ; ledger public pour fixer ça en v4 |

**Légende** : ✅ détecté · 🟡 partiellement résistant · ❌ non détecté

---

## 3. Analyse détaillée des attaques principales

### 3.1 Attaque #6 — Bruteforce coordonné sur les témoins (v1 vs v2)

**Scénario** : un fraudeur veut faire passer une fausse claim. Il a calculé V faux et veut tomber par hasard sur des (M, T, P*) qui matchent.

**v1** :
- 3 témoins, chacun ∈ ℤ/7ℤ → 7³ = 343 combinaisons.
- Probabilité d'un match aléatoire : 1/343 ≈ 0.29%
- Avec 100 essais : ~25% de réussite par session.

**v2** :
- 5 témoins : (M, T) chacun mod 7, (P7, P11, P13) sur 7·11·13.
- M et T sont triviaux à recalculer pour le fraudeur (sum/count) → pas de protection sur eux.
- (P7, P11, P13) sur le V réel : 1/(7·11·13) = 1/1001 ≈ 0.1%
- Avec 100 essais : ~9% de réussite.

**Gain v2** : ~3× plus difficile à brute-forcer. Pour avoir 50% de réussite : 7 essais en v1 vs 700 en v2.

**Note** : pour passer à 1/(7·11·13·17·19·23) ≈ 1/10⁸, ajouter 3 modulus supplémentaires (v3 prévue).

---

### 3.2 Attaque #7 — Swap de formule (la grande nouveauté v2)

**Scénario** : un attaquant possède une claim honnête `(formula_A, prop_A)` qui attest. Il veut la transformer en `(formula_B, prop_A)` où formula_B fait dire ce qu'il veut.

**v1** :
- prop_A est valide indépendamment (sceau 7 = sum i0..i5 mod 7).
- formula_B est valide indépendamment (passe ses 4 phases si elle est arithmétiquement correcte).
- VERITAS-v1 : ATTESTEE. **Attaque réussie.**

**v2** :
- prop_A a un i6 calculé pour `formula_hash(formula_A) = fhash_A`.
- Avec formula_B, le verdict recompute `fhash_B = formula_hash(formula_B) ≠ fhash_A`.
- veritas_seal_7 vérifie : `i6 == (sum + fhash_B) mod 7` mais i6 a été calculé pour fhash_A → FAIL.
- VERITAS-v2 : `BRISEE AU RAISONNEMENT`. **Attaque détectée.**

**Démonstration empirique** (eval Aether sur v2) :

```
Claim originale : (3+4)·5=35, fhash=4, prop i6=5 → ATTESTEE
Claim swappée   : 2¹⁰=1024 (fhash=5) + ancienne prop (i6=5 pour fhash=4) → BRISEE AU RAISONNEMENT
```

L'attaquant a deux options :
1. **Garder la prop** → la sceau 7 chute (cas ci-dessus).
2. **Recalculer i6 pour fhash_B** → la claim est techniquement valide, mais c'est **une nouvelle claim distincte** avec son propre fhash. Couplée à un timestamp public (v4), la substitution sera rétroactivement détectée.

---

### 3.3 Attaque #2 — Hallucination IA (le scénario typique)

**Scénario** : une IA co-rédige un papier. Elle hallucinait `(3+4)·5 = 30` et embed P7=2 (= 30 mod 7).

```
Claim : (list 3 4 1001 5 1003   5 2 2 _ _   prop)
                                    ^^^ 2 = 30 mod 7 (hallucination)
```

**Détection** :
1. VERITAS recompute V depuis l'expression `(3+4)*5` → V réel = 35.
2. PHARES vérifie 35 mod 7 = 0 ≠ 2 (le P7 menteur).
3. Verdict : `BRISEE A LA FORMULE`.

L'IA a été démasquée **sans oracle externe**. Le coût : l'auteur humain a juste fourni la claim.

---

### 3.4 Attaques non couvertes (limites v2)

#### 3.4.1 Data fabrication (Attaque #14)
Si l'auteur fabrique les données puis calcule correctement dessus, VERITAS atteste : le calcul est juste **sur les données fournies**. Le witness du dataset n'existe pas en v2.

**Mitigation v3** : ajouter un 6ᵉ témoin Δ = hash mod 7 du dataset (mean, variance, n, hash de fichier brut).

#### 3.4.2 Backdating (Attaque #15)
VERITAS atteste qu'une claim est consistente, pas qu'elle a été pré-enregistrée à une date donnée. Un fraudeur peut construire la claim après les données.

**Mitigation v4** : ancrage dans un Merkle tree public (Git, blockchain, ou registry dédié) avec horodatage cryptographique.

#### 3.4.3 Compromission du registre (Attaque #16)
Si l'attaquant a accès au système de stockage des claims pré-enregistrées, il peut substituer post-hoc.

**Mitigation v4** : ledger immuable (blockchain Bitcoin/Ethereum, ou registre Git public avec signatures).

---

## 4. Garanties cryptographiques

VERITAS-v2 **n'offre pas** :
- Confidentialité (les claims sont en clair)
- Non-répudiation forte (pas de signature numérique)
- Sécurité computationnelle prouvée (fhash n'est pas un MAC)

VERITAS-v2 **offre** :
- **Intégrité par construction** : l'auteur doit pré-calculer V mod 7/11/13 et fhash, ce qui exige le calcul réel
- **Couplage formule↔raisonnement** : la prop est inséparable de la formule à laquelle elle a été authorée
- **Rejet par phase précise** : indique exactement quelle dimension est défaillante

C'est suffisant pour le contexte cible (publication scientifique honnête avec détection d'erreurs/fraudes naïves), mais **insuffisant pour des contextes adversariaux extrêmes** (fraudes coordonnées, état-nation).

---

## 5. Roadmap sécurité

| Item | Tier | Cible |
|---|:-:|---|
| Witness du dataset (Δ) | 1 | v3 |
| Multi-mod additionnel (17, 19, 23) | 2 | v3 |
| Horodatage cryptographique externe | 1 | v4 |
| Signature ed25519 de l'auteur | 2 | v4 |
| ZK-proof du raisonnement | 3 | v5 (recherche) |

---

## 6. Réponse à la question : « VERITAS est-il sûr ? »

**Oui, contre l'adversaire qu'il vise** : auteur maladroit, IA hallucinante, fraudeur naïf. Pour ces 3 profils, VERITAS-v2 offre une détection **quasi-déterministe**.

**Non, contre un adversaire computationnel équipé** : 1/1001 de collision est trivialement bruteforcable avec 10000 essais (~9 heures sur un laptop). Pour cet usage, attendre v3.

**Inadéquat pour des contextes adversariaux extrêmes** : préférer des outils cryptographiques standards (Merkle, ZK-proofs) en complément.
