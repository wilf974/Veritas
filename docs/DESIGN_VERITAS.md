# DESIGN — VERITAS v2

**Auteur :** Wilfred Maillot
**Date :** 2026-05-07
**Statut :** spec figée

---

## 1. Problème adressé

La science moderne souffre de la **crise de reproductibilité** :
- ~36% des études en psychologie répliquent (OSF 2015)
- ~11% des études précliniques en oncologie répliquent (Begley & Ellis 2012)
- Coût estimé : ~28 milliards $/an (Freedman et al. 2015) pour la biomédecine US seule

Les causes principales :
- **HARKing** (Hypothesizing After Results are Known)
- **p-hacking** (multiplication de tests jusqu'à p < 0.05)
- **Erreurs arithmétiques** non détectées
- **Hallucinations** de co-auteurs IA
- **Fabrication de données** (Stapel-style)

Les outils existants (OSF pre-registration, AsPredicted, journal data-policies) sont **déclaratifs** — l'auteur doit honnêtement décrire sa méthode. Aucun n'oblige une **auto-prédiction cryptographique du résultat**.

---

## 2. Principe central

Une **claim VERITAS** est une donnée auto-contenue, structurée pour exiger de son auteur deux choses :

1. **Calculer correctement** un résultat numérique (et embarquer 5 témoins de cette correction)
2. **Structurer rigoureusement** son raisonnement en 7 dimensions vérifiables

Les deux parties sont **liées cryptographiquement** : l'une ne peut être substituée ou modifiée sans casser l'autre.

> *Deutéronome 19:15 — « Sur la parole de deux ou de trois témoins, l'affaire sera décidée. »*

---

## 3. Format d'une claim v2

```
claim = (list  <expr-tokens>  M  T  P7  P11  P13   i0 i1 i2 i3 i4 i5 i6)
              \------- formule MTP-v2 -------/    \--- proposition ---/
              len-7 ≥ 6                            exactement 7
```

**Total ≥ 13 tokens.**

### 3.1 Tokens de l'expression

| Plage | Sens |
|---|---|
| `0..999` | opérandes (les nombres eux-mêmes) |
| `1001..1010` | opérateurs binaires |
| `1101..1199` | opérateurs unaires |

### 3.2 Table des opérateurs

| Code | Symbole | Arité | Action |
|---:|---|---|---|
| 1001 | + | 2 | addition |
| 1002 | − | 2 | soustraction |
| 1003 | × | 2 | multiplication |
| 1004 | ÷ | 2 | division entière (`floor`) — sentinelle si b=0 |
| 1005 | % | 2 | modulo — sentinelle si b=0 |
| 1006 | ^ | 2 | puissance |
| 1009 | min | 2 | minimum |
| 1010 | max | 2 | maximum |
| 1101 | √ | 1 | racine carrée |
| 1102 | \|·\| | 1 | valeur absolue |

Sentinelle d'erreur : `-999999`, propagée par `apply_binary` et `apply_unary`.

### 3.3 Daniel-Pentade (5 témoins)

| Témoin | Définition | Sens biblique |
|---|---|---|
| **M** | `(somme des opérandes) mod 7` | « tu es **compté** » |
| **T** | `(nb d'opérateurs) mod 7` | « tu es **pesé** » |
| **P7** | `V mod 7` | « tu es **divisé** » par 7 |
| **P11** | `V mod 11` | par 11 |
| **P13** | `V mod 13` | par 13 |

Probabilité qu'un V faux match (P7, P11, P13) par hasard : **1/(7·11·13) = 1/1001** (~0,1 %), contre 1/7 (~14 %) en v1.

### 3.4 Proposition (7 features)

Identique aux Sept Sceaux v1 :

| Index | Sens | Sceau qui le lit |
|:-:|---|---|
| `i0` | longueur d'intention (>0) | 1 — Logos |
| `i1` | nb sous-parties (≥2) | 2 — Dualité |
| `i2` | accord trinitaire (=1) | 3 — Trinité |
| `i3` | bitmask 4-coins (=15) | 4 — Quatre Coins |
| `i4` | sondes (≥5) | 5 — La Main |
| `i5` | score d'imperfection (<6) | 6 — L'Imparfait |
| `i6` | checksum **cross-linké** | 7 — Sabbat v2 |

---

## 4. Innovations v2 par rapport à v1

### 4.1 Daniel-Pentade

V1 utilisait (M, T, P) sur ℤ/7ℤ → collision 1/343 (1/7³).
V2 utilise (M, T, P7, P11, P13) → collision **1/1001** sur les seuls témoins de V.

**Pourquoi cibler V** : M et T sont triviaux à calculer pour un attaquant qui modifie l'expression. La protection coûteuse doit aller sur le résultat lui-même.

### 4.2 Cross-link formule↔raisonnement

V1 : sceau 7 = `i6 = (sum i0..i5) mod 7` — proposition indépendante.
V2 : sceau 7 = `i6 = (sum i0..i5 + fhash) mod 7`

où `fhash = (M + T + P7 + P11 + P13) mod 7`.

**Conséquence** : si on substitue la formule, fhash change, et la proposition (qui a un i6 calculé pour l'ancien fhash) se brise au sceau 7. Le couple est **inseparable**.

> *Romains 7:2 — « La femme mariée est liée par la loi à son mari tant qu'il est vivant. »*

---

## 5. Les 7 sceaux + 4 phases

### 5.1 Sceaux (proposition)

| Sceau | Symbole | Vérification | Référence |
|:-:|---|---|---|
| 1 | Logos | i0 > 0 | Jean 1:1 |
| 2 | Dualité | i1 ≥ 2 | Genèse 1:6 |
| 3 | Trinité | i2 = 1 | Mt 28:19 |
| 4 | Quatre Coins | i3 = 15 (0b1111) | Apoc 7:1 |
| 5 | La Main | i4 ≥ 5 | Exode 14:21 |
| 6 | L'Imparfait | i5 < 6 | Apoc 13:18 |
| 7 | Sabbat v2 | i6 = (sum i0..i5 + fhash) mod 7 | Genèse 2:2-3 |

### 5.2 Phases (formule)

| Phase | Symbole | Vérification | Référence |
|:-:|---|---|---|
| 1 | MANE I | M = (sum opérandes) mod 7 | Daniel 5:25 |
| 2 | MANE II | T = (nb opérateurs) mod 7 | Daniel 5:25 |
| 3 | TEKEL | RPN evaluation → V | Daniel 5:27 |
| 4 | PHARES | V mod 7=P7, V mod 11=P11, V mod 13=P13 | Daniel 5:28 |

### 5.3 Composition

```
                    Une CLAIM
                        │
              ┌─────────┴─────────┐
              ▼                   ▼
       Formule MTP v2        Proposition
              │                   │
        is_resolved_v2     veritas_open_seals(p, fhash)
              │                   │
       formula_attests   proposition_attests
              │                   │
              └─── attestation_level (0..3) ───┐
                                                ▼
                                       veritas_verdict
                                          (str)
```

### 5.4 Niveaux d'attestation

| Niveau | Formule | Raisonnement | Verdict |
|:-:|:-:|:-:|---|
| 3 | ✅ | ✅ | `ATTESTEE` |
| 2 | ❌ | ✅ | `BRISEE A LA FORMULE` |
| 1 | ✅ | ❌ | `BRISEE AU RAISONNEMENT` |
| 0 | ❌ | ❌ | `BRISEE AUX DEUX` |

**Note v2** : à cause du cross-link, les cas 1 et 2 deviennent rares. Si la formule est tampered, fhash change, ce qui casse aussi la proposition. La plupart des attaques tomberont en niveau 0 (`BRISEE AUX DEUX`).

---

## 6. Choix de conception

| Décision | Raison |
|---|---|
| **Encodage numérique pur** | Aether traite mieux les listes homogènes ; invariants se formulent sur des nombres |
| **Mod 7 pour M/T et le sceau 7** | Fidélité biblique (perfection) et taille de hash légère pour des structures (M, T) |
| **Mod 11/13 pour V** | Multi-prime augmente la sécurité de la valeur exponentiellement |
| **Sentinelle au lieu de Result type** | Aether stdlib offre `result:ok` mais nécessite un flag ; la sentinelle simple suffit |
| **`reduce:list` au lieu de `while`** | Préserve recursion = none ; pur fonctionnel |
| **Sceau 7 cross-linké** | Ferme l'attaque par swap formule/proposition |
| **fhash sur les 5 témoins** | L'engagement local est suffisant — pas besoin d'un hash cryptographique vrai pour ce niveau de menace |
| **Proposition séparée** (7 derniers tokens) | Préserve la modularité — un même format de proposition pour différents domaines |

---

## 7. Hors-scope (v2)

- Pas d'opérandes négatifs (le comportement de `% a 7` pour `a < 0` est non-spécifié dans Aether)
- Pas de flottants (encodage **fixed-point** prévu en v3)
- Pas de cryptographie réelle (fhash n'est pas un MAC ; c'est un engagement structurel)
- Pas d'horodatage (pré-registration externe via Merkle tree prévue en v4)
- Pas de bundle multi-claims (prévu en v3)
- Inputs supposés bien formés : `(>= (len claim) 13)`

---

## 8. Critères de réussite

- ✅ `validate` passe sans erreur
- ✅ `verify` : 25/25 user examples
- ✅ `counter_example` : 0 violation sur ≥ 8 invariants
- ✅ `explain` : recursion = none partout
- ✅ Hallucination détectée à PHARES sans oracle externe
- ✅ Swap de formule détecté à Sabbat v2 (cross-link)
- ✅ Tag `veritas-v2` posé

---

## 9. Continuité avec les algorithmes prédécesseurs

```
sept-sceaux-v1 ──┐
                 ├─→ veritas-v1 (composition simple)
mtp-v1       ──┘                   │
                                    ▼
                             veritas-v2 (cross-link + multi-témoins)
```

Les deux modules sources restent indépendamment utilisables (sept-sceaux pour structure-only, mtp pour numérique-only). VERITAS-v2 les combine en redéfinissant le sceau 7.
