# CHANGELOG

Toutes les évolutions notables de VERITAS sont consignées ici.
Le format suit [Keep a Changelog](https://keepachangelog.com/fr/) et le projet adhère à [SemVer](https://semver.org).

---

## [v2] — 2026-05-07

### Ajouts (feat)
- **Daniel-Pentade** : extension du Triplet de Daniel à 5 témoins `(M, T, P7, P11, P13)`. La valeur V est maintenant attestée sur 3 modules (7, 11, 13).
- **Cross-link** : nouveau sceau 7 (`veritas_seal_7`) qui intègre `formula_hash` dans son checksum. La proposition est scellée à la formule.
- `formula_hash(f)` : nouvelle fonction publique calculant l'engagement local d'une formule.
- `veritas_open_seals(p, fhash)` : remplace `is_sealed` du composeur v1, prend désormais le fhash en argument.
- `veritas_seal_7(p, fhash)` : nouveau sceau 7 cross-linké.
- 5 nouvelles fonctions d'accès aux témoins multi-modulus : `witness_P_7`, `witness_P_11`, `witness_P_13` (`witness_M_v2` et `witness_T_v2` remplacent leurs versions v1).
- `phares_v2`, `phases_passed_v2`, `is_resolved_v2`, `mane_I_v2`, `mane_II_v2` : versions adaptées aux 5 témoins.

### Modifications (changed)
- **Format de claim** : passe de `expr + 3 témoins + 7 sceaux` (≥ 11 tokens) à `expr + 5 témoins + 7 sceaux` (≥ 13 tokens). **BREAKING**.
- **`expr_part`** → `expr_part_v2` : slice les `len-5` premiers tokens (au lieu de `len-3`).
- **Sceau 7** : sceau autonome (`seal_7_sabbat` v1) → sceau cross-linké (`veritas_seal_7`).
- Probabilité de collision aléatoire sur V : `1/7³ = 1/343` → `1/(7·11·13) = 1/1001` (**~3× plus sûr**).

### Sécurité (security)
- **Attaque par swap de formule** désormais détectée à Sabbat v2 (au lieu d'être laissée passer en v1).
- **Multi-mod sur V** rend les attaques par brute-force (chercher une formule qui matche tous les témoins) ~3× plus chères.

### Documentation (docs)
- Nouveau **README.md** (vue d'ensemble du projet)
- Nouveau **docs/DESIGN.md** (architecture détaillée)
- Nouveau **docs/API.md** (référence des 27 fonctions)
- Nouveau **docs/CHANGELOG.md** (ce fichier)
- Nouveau **docs/THREAT_MODEL.md** (modèle de menace + résistance par attaque)

### Tests
- 25 user examples (vs 16 en v1)
- 17 invariants user déclarés
- counter_example : 0 violation, jusqu'à **104 probes** sur `veritas_seal_7`
- Démo de cross-link : swap de formule détecté à Sabbat v2

### Note de migration v1 → v2
**Les claims v1 ne sont pas compatibles avec v2.** Les formules en v1 ont 3 témoins suffixes ; celles en v2 en ont 5. Pour migrer une claim :
1. Calculer P11 = V mod 11 et P13 = V mod 13
2. Recalculer fhash = (M + T + P7 + P11 + P13) mod 7
3. Recalculer i6 (sabbat v2) = (sum i0..i5 + fhash) mod 7
4. Reconstruire la claim avec le nouveau format

---

## [v1] — 2026-05-07

### Ajouts initiaux
- Composition de **sept-sceaux-v1** (raisonnement) et **mané-thékel-pharès-v1** (formule).
- 35 fonctions Aether (10 sceaux + 19 mtp + 6 wrappers).
- Format claim : `expr + (M T P) + (i0 i1 i2 i3 i4 i5 i6)` (≥ 11 tokens).
- Verdicts : `ATTESTEE` / `BRISEE A LA FORMULE` / `BRISEE AU RAISONNEMENT` / `BRISEE AUX DEUX`.
- 4 claims canoniques de démo, tous retournant le verdict attendu.

### Limitations connues v1 (résolues en v2)
- Collision aléatoire de 1/343 (un seul modulus 7)
- Pas de cross-link : un attaquant peut substituer la formule en gardant la proposition d'une claim honnête
- Daniel-Triplet sur 3 dimensions (M, T, P) toutes mod 7
