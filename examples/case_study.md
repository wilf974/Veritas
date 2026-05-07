# Cas d'étude : RCT Mindfulness — Pipeline complet du stack

> *Application bout-en-bout du stack Joseph → Pentecôte → Tribunal sur un papier scientifique réaliste avec 10 claims numériques.*

**Date :** 2026-05-07
**Pipeline testé :** `joseph-v1` → `pentecote-v1` → `tribunal-v1`
**Verdict global :** **2 claims ACQUITTÉES, 8 claims CONDAMNÉES** sur 5 objections différentes.

---

## 1. Le papier soumis

> **Effet d'un programme structuré de pleine conscience de 12 semaines sur le stress professionnel : essai randomisé contrôlé multi-centrique**
> *(Auteurs imaginaires, étude synthétique mais réaliste construite pour le cas d'étude)*

### Méthodologie

- **Design** : RCT à 2 bras avec liste d'attente comme contrôle
- **N total** : 2 000 employés (1 000 par bras), 12 entreprises
- **Pré-enregistrement** : oui (registre clinical-trials)
- **Outcome principal** : Score PSS-10 (Perceived Stress Scale) à 12 semaines
- **Outcomes secondaires** : burnout (MBI), satisfaction de vie, productivité auto-rapportée
- **Suivi** : 12 semaines + sous-échantillon n=600 à 6 mois

### Résultats principaux annoncés

> Le programme réduit le stress de 30 % (p<0.001) et le burnout de 25 % (p=0.005). Effet plus fort chez les femmes (40 %) et les ≥45 ans (35 %). 70 % des participants terminent le programme. Effet persiste à 6 mois sur le sous-échantillon. Corrélation positive entre temps de méditation et réduction de stress. Coût par employé : 200 €. Aucun événement indésirable sérieux. **Conclusion : tous les employeurs devraient adopter ce programme.**

---

## 2. Extraction des 10 claims

| # | Claim | Champ critique |
|:-:|---|---|
| 1 | Réduction PSS de 30 % à 12 sem (p<0.001) | endpoint primaire |
| 2 | Réduction burnout de 25 % à 12 sem (p=0.005) | endpoint secondaire |
| 3 | Effet plus fort chez les femmes (n=1000, +40 %) | sous-groupe |
| 4 | Effet en ≥45 ans (n=400, +35 %) | sous-groupe |
| 5 | Engagement : 70 % terminent le programme | descriptif |
| 6 | Effet persiste à 6 mois (n=600 sous-échantillon) | suivi long |
| 7 | Corrélation temps de méditation / réduction stress | mécanisme |
| 8 | Coût-efficacité 200 €/employé pour bénéfice mesuré | économique |
| 9 | Pas d'événement indésirable sérieux | sécurité |
| 10 | « Tous les employeurs devraient adopter ce programme » | recommandation universelle |

---

## 3. Encodage Tribunal de chaque claim

Format : `(list n p_x10000 es_x100 n_alt rep data causal pent_status)`

| # | Encoding | Justification |
|:-:|---|---|
| 1 | `(list 2000 10 30 0 4 1 2 3)` | n=2000 ; p=0.001 ; ES=0.30 ; pas d'alt ; rep=4 (RCT pré-enregistré non répliqué) ; data=collecté ; causal=expérimental ; Pentecôte PARFAITE |
| 2 | `(list 2000 50 25 0 4 1 2 3)` | p=0.005 ; ES=0.25 (le reste idem) |
| 3 | `(list 1000 100 40 1 3 1 2 3)` | sous-groupe → 1 hypothèse alt (différence pré-existante) ; rep=3 (subgroup non pré-enregistré séparément) |
| 4 | `(list 400 200 35 1 3 1 2 3)` | n=400 (subgroup) ; p=0.02 |
| 5 | `(list 1000 50 50 0 5 1 1 3)` | descriptif simple, causal=corrélation |
| 6 | `(list 600 100 25 1 3 1 2 3)` | sous-échantillon n=600, rep=3 (un seul follow-up) |
| 7 | `(list 2000 50 30 1 4 1 1 3)` | corrélation, causal=1 |
| 8 | `(list 2000 200 10 3 3 1 1 3)` | analyse coût : 3 méthodologies alternatives plausibles ; rep=3 |
| 9 | `(list 2000 5000 5 1 4 1 2 3)` | non-rejet H₀ avec p=0.5 (pas de différence détectée mais power non discuté) |
| 10 | `(list 2000 10 30 2 4 1 2 3)` | recommandation universelle : 2 hypothèses alt évidentes (autres interventions, biais de sélection des participants) |

---

## 4. Verdicts du Tribunal

| # | Claim | **Verdict** |
|:-:|---|---|
| 1 | Réduction PSS 30 % à 12 sem | **`ACQUITTEE`** |
| 2 | Réduction burnout 25 % | **`ACQUITTEE`** |
| 3 | Effet femmes (n=1000) | `CONDAMNEE PAR REPRODUCTIBILITE` |
| 4 | Effet ≥45 ans (n=400) | `CONDAMNEE PAR GENERALISATION` |
| 5 | Engagement 70 % | `CONDAMNEE PAR CAUSALITE` |
| 6 | Effet à 6 mois | `CONDAMNEE PAR REPRODUCTIBILITE` |
| 7 | Corrélation dose-réponse | `CONDAMNEE PAR CAUSALITE` |
| 8 | Coût-efficacité | `CONDAMNEE PAR ALTERNATIVE` |
| 9 | Pas d'EI sérieux | `CONDAMNEE PAR METHODE` |
| 10 | « Tous les employeurs » | `CONDAMNEE PAR ALTERNATIVE` |

**Distribution :**

```
ACQUITTEE                          : 2  (20 %)
CONDAMNEE PAR CAUSALITE            : 2  (20 %)
CONDAMNEE PAR GENERALISATION       : 1  (10 %)
CONDAMNEE PAR REPRODUCTIBILITE     : 2  (20 %)
CONDAMNEE PAR METHODE              : 1  (10 %)
CONDAMNEE PAR ALTERNATIVE          : 2  (20 %)
```

---

## 5. Détail par claim — Sévérités complètes

### Claim 1 — Réduction PSS de 30 % (PRIMARY)

| Objection | Sévérité | Lecture |
|---|:-:|---|
| Calcul | 0 | Pentecôte PARFAITE |
| Données | 0 | collecté |
| Causalité | 3 | expérimental (RCT) |
| Généralisation | 0 | n=2000 ≥ 1500 |
| Méthode | 0 | p=0.001 ≤ 0.001 |
| Alternative | 0 | endpoint pré-spécifié |
| Reproductibilité | 3 | rep=4 (un seul RCT) |

**Max : 3 < 4 → `ACQUITTEE`**

→ Le findings principal résiste à toutes les objections. C'est la claim la plus solide du papier.

---

### Claim 2 — Réduction burnout de 25 % (SECONDARY)

Identique à Claim 1 mais p=0.005 (sév 2). Max=3 → **`ACQUITTEE`**.

---

### Claim 3 — Effet plus fort chez les femmes (n=1000)

| Objection | Sévérité |
|---|:-:|
| Causalité | 3 |
| Généralisation | 3 (1000 ≤ n < 1500) |
| Reproductibilité | **4** (rep=3 → sév 4) |
| Alternative | 2 (1 alt) |
| Autres | 0..2 |

**Max : 4 → `CONDAMNEE PAR REPRODUCTIBILITE`**

→ Subgroup analysis non pré-enregistrée séparément. Le Tribunal demande : « cet effet a-t-il été répliqué dans une étude indépendante ? » Réponse : non.

---

### Claim 4 — Effet en ≥45 ans (n=400)

| Objection | Sévérité |
|---|:-:|
| Généralisation | **5** (150 ≤ n < 500) |
| Reproductibilité | 4 |
| Méthode | 5 (p=0.02) |
| Causalité | 3 |
| Autres | ≤2 |

**Max : 5 → `CONDAMNEE PAR GENERALISATION`** (priorité d'index sur méthode tied)

→ Sous-groupe trop petit pour conclure. Effet réel ou bruit ?

---

### Claim 5 — Engagement 70 %

| Objection | Sévérité |
|---|:-:|
| Causalité | **5** (causal=1, descriptif) |
| Généralisation | 3 |
| Autres | ≤3 |

**Max : 5 → `CONDAMNEE PAR CAUSALITE`**

→ Statistique descriptive sans claim causale possible. Le Tribunal note que c'est **un fait observé**, pas une découverte scientifique.

---

### Claim 6 — Effet à 6 mois (n=600)

| Objection | Sévérité |
|---|:-:|
| Reproductibilité | **4** (rep=3 → 4) |
| Causalité | 3 |
| Méthode | 2 |
| Autres | ≤2 |

**Max : 4 → `CONDAMNEE PAR REPRODUCTIBILITE`**

→ Suivi unique sur sous-échantillon. Effet à long terme à confirmer.

---

### Claim 7 — Corrélation dose-réponse

| Objection | Sévérité |
|---|:-:|
| Causalité | **5** (causal=1) |
| Reproductibilité | 3 |
| Autres | ≤2 |

**Max : 5 → `CONDAMNEE PAR CAUSALITE`**

→ Plus de méditation = moins de stress, mais l'inverse est aussi plausible (les moins stressés méditent davantage). **C'est l'objection la plus classique en science.**

---

### Claim 8 — Coût-efficacité 200 €/employé

| Objection | Sévérité |
|---|:-:|
| Alternative | **6** (alt=3, plusieurs méthodologies coût-efficacité divergent) |
| Causalité | 5 |
| Reproductibilité | 4 |
| Méthode | 5 |
| Autres | ≤3 |

**Max : 6 → `CONDAMNEE PAR ALTERNATIVE`**

→ L'analyse économique dépend des hypothèses prises (coût opportunité, valorisation du temps). Le Tribunal rappelle : trois méthodes différentes donneraient trois résultats différents.

---

### Claim 9 — Pas d'événement indésirable sérieux

| Objection | Sévérité |
|---|:-:|
| Méthode | **7** (p=0.5, absence de power) |
| Reproductibilité | 3 |
| Autres | ≤3 |

**Max : 7 → `CONDAMNEE PAR METHODE`**

→ « Pas trouvé » ≠ « N'existe pas ». Avec n=2000, l'étude n'a pas la puissance statistique pour détecter des EI rares (< 1/1000). **Erreur classique : confondre absence de preuve et preuve d'absence.**

---

### Claim 10 — « Tous les employeurs devraient adopter »

| Objection | Sévérité |
|---|:-:|
| Alternative | **4** (n_alt=2 : autres interventions équivalentes possibles, biais de sélection des entreprises participantes) |
| Causalité | 3 |
| Autres | ≤3 |

**Max : 4 → `CONDAMNEE PAR ALTERNATIVE`**

→ La recommandation universelle ignore les hypothèses concurrentes : (a) d'autres interventions (sport, sommeil, charge de travail) pourraient avoir un effet similaire ; (b) les entreprises ayant accepté de participer sont possiblement plus favorables au bien-être que la moyenne.

---

## 6. Rapport final — Style peer review

### Synthèse pour les éditeurs

> **Verdict global : papier méthodologiquement solide sur ses endpoints primaires (claims 1-2 ACQUITTÉES), mais ses 8 autres claims ne survivent pas à un audit Tribunal systématique.**

### Forces du papier

1. **Endpoint primaire robuste** : la réduction du stress de 30 % (n=2000, p<0.001) résiste à toutes les objections (claim 1).
2. **Endpoint secondaire cohérent** : burnout également ACQUITTÉ (claim 2).
3. **Pré-enregistrement** : limite le p-hacking sur les endpoints primaires.
4. **Données collectées proprement** : aucune objection sur la donnée brute (donnees=0 partout).
5. **Calcul auto-attesté** : Pentecôte PARFAITE sur les 10 claims (calcul=0).

### Faiblesses majeures

1. **Sous-groupes non pré-enregistrés** (claims 3-4) : effets sur femmes et ≥45 ans pourraient être du data mining.
2. **Affirmations causales sur des corrélations** (claims 5, 7) : engagement et dose-réponse ne peuvent pas être interprétés causalement sans manipulation expérimentale.
3. **Suivi long-terme limité** (claim 6) : sous-échantillon unique non répliqué.
4. **Analyse économique fragile** (claim 8) : dépend des hypothèses prises ; trois méthodologies alternatives donneraient trois résultats divergents.
5. **Sécurité non démontrée** (claim 9) : « pas d'EI sérieux » ≠ « sécurité prouvée ». Power statistique insuffisant pour les EI rares.
6. **Recommandation universelle non défendable** (claim 10) : les hypothèses concurrentes ne sont pas écartées.

### Demandes au(x) auteur(s)

| # | Claim | Demande spécifique |
|:-:|---|---|
| 3 | Sous-groupe femmes | Pré-enregistrer dans la prochaine étude OU rétrograder en exploratoire |
| 4 | Sous-groupe ≥45 ans | Pareil + augmenter n |
| 5 | Engagement | Reformuler en « observation descriptive » sans claim causale |
| 6 | Effet à 6 mois | Réplication indépendante requise avant publication de cette claim |
| 7 | Corrélation dose-réponse | Discuter explicitement la causalité inverse possible |
| 8 | Coût-efficacité | Présenter ≥3 analyses sensitivity avec hypothèses divergentes |
| 9 | Sécurité | Calculer puissance statistique pour détecter EI à 1/1000 ; admettre limites |
| 10 | « Tous les employeurs » | Restreindre à la population étudiée ; pas de généralisation universelle |

### Décision recommandée

**Major revision** (révision majeure). Les 2 claims primaires sont publiables ; les 8 autres requièrent reformulation, ajout de nuances, ou retrait.

---

## 7. Discussion : ce que le Tribunal apporte

### Sans Tribunal

L'éditeur reçoit le papier, lit l'abstract :

> *« RCT n=2000, p<0.001, réduction stress 30 %. Tous les employeurs devraient adopter. »*

Il pourrait l'accepter. Pentecôte dirait PARFAITE pour le calcul.

### Avec Tribunal

L'éditeur applique mécaniquement les 7 objections sur les 10 claims. **2 sont ACQUITTÉES, 8 sont nuancées ou rejetées**. Le rapport ci-dessus est généré en 0.001 seconde.

L'auteur reçoit une liste précise d'amendements à apporter. Le papier final est :
- **honnête** sur ce qui est démontré et ce qui ne l'est pas
- **utilisable** par les médecins et les RH
- **réplicable** par d'autres équipes

### Le saut conceptuel

| | Sans Tribunal | Avec Tribunal |
|---|---|---|
| Question posée | Est-ce cohérent ? | Est-ce défendable ? |
| Sortie | « Oui / Non » | « Solide sur 1-2, fragile sur 3-10, voici pourquoi » |
| Reviewer humain | doit tout chercher | se concentre sur le contenu, pas la chasse aux failles |
| Auteur | reçoit un rejet vague | reçoit un plan d'amendement précis |

---

## 8. Limites du cas d'étude

1. **Synthétique** : l'étude n'existe pas, les chiffres sont plausibles mais inventés.
2. **Encoding subjectif** : décider que « la corrélation dose-réponse » a `causal=1` est un jugement humain. Le Tribunal automatise la conséquence, pas la classification.
3. **Tribunal v1 ne capture pas tout** : la sub-question « ce protocole est-il adapté à un domaine business ? » n'est pas dans les 7 objections.

Ces limites sont des **invitations** à v2 — pas des défauts. Le système actuel **est utile dès maintenant**.

---

## 9. Conclusion

Sur 10 claims numériques d'un papier réaliste :
- **2 ACQUITTÉES** (les endpoints primaires bien faits)
- **8 nuancées par 5 objections différentes** (sous-groupes, causalité, suivi long, coût, sécurité, généralisation)

Le Tribunal ne dit pas « ce papier est faux ». Il dit :

> Ce papier établit deux résultats solides. Les huit autres claims doivent être reformulées ou démontrées plus rigoureusement.

C'est la **vraie peer review**. Et c'est mécanisable.

---

## Commande pour reproduire

```aether
;; Charger tribunal.aether dans Aether MCP
;; Évaluer les 10 claims :

(list
  (call tribunal_verdict (list 2000 10 30 0 4 1 2 3))
  (call tribunal_verdict (list 2000 50 25 0 4 1 2 3))
  (call tribunal_verdict (list 1000 100 40 1 3 1 2 3))
  (call tribunal_verdict (list 400 200 35 1 3 1 2 3))
  (call tribunal_verdict (list 1000 50 50 0 5 1 1 3))
  (call tribunal_verdict (list 600 100 25 1 3 1 2 3))
  (call tribunal_verdict (list 2000 50 30 1 4 1 1 3))
  (call tribunal_verdict (list 2000 200 10 3 3 1 1 3))
  (call tribunal_verdict (list 2000 5000 5 1 4 1 2 3))
  (call tribunal_verdict (list 2000 10 30 2 4 1 2 3)))
```

Sortie :
```
("ACQUITTEE" "ACQUITTEE" "CONDAMNEE PAR REPRODUCTIBILITE"
 "CONDAMNEE PAR GENERALISATION" "CONDAMNEE PAR CAUSALITE"
 "CONDAMNEE PAR REPRODUCTIBILITE" "CONDAMNEE PAR CAUSALITE"
 "CONDAMNEE PAR ALTERNATIVE" "CONDAMNEE PAR METHODE"
 "CONDAMNEE PAR ALTERNATIVE")
```

---

**Auteur :** Wilfred Maillot
**Date :** 2026-05-07
