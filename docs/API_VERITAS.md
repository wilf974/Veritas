# API — VERITAS v2

Référence complète des 27 fonctions de `veritas_v2.aether`. Toutes pures, non-récursives, déterministes.

---

## Index

- [Sceaux atomiques](#sceaux-atomiques) (7 fns)
- [Helpers d'accès MTP](#helpers-daccès-mtp) (5 fns)
- [Helpers de comptage](#helpers-de-comptage) (4 fns)
- [Phases MTP](#phases-mtp) (3 fns + 4 helpers)
- [Évaluation RPN](#évaluation-rpn) (4 fns)
- [Orchestration MTP](#orchestration-mtp) (2 fns)
- [VERITAS](#veritas) (7 fns)

---

## Sceaux atomiques

Vérifient une dimension du raisonnement structuré.

### `seal_1_logos((p list)) → bool`
**Sceau 1 — Logos** *(Jean 1:1 « Au commencement était le Verbe »)*
Vrai ssi `i0 > 0`. Vérifie qu'une intention non-vide est déclarée.
**Invariant** : `(= result (> (get p 0) 0))`

### `seal_2_dualite((p list)) → bool`
**Sceau 2 — Dualité** *(Genèse 1:6, séparation eaux/eaux)*
Vrai ssi `i1 ≥ 2`. Vérifie une décomposition en au moins 2 sous-parties.

### `seal_3_trinite((p list)) → bool`
**Sceau 3 — Trinité** *(Mt 28:19)*
Vrai ssi `i2 = 1`. Vérifie l'accord trinitaire (3 vérifications indépendantes convergent).

### `seal_4_coins((p list)) → bool`
**Sceau 4 — Quatre Coins** *(Apoc 7:1)*
Vrai ssi `i3 = 15`. Bitmask des 4 cas-limites couverts (vide, max, négatif, normal).

### `seal_5_main((p list)) → bool`
**Sceau 5 — La Main** *(Exode 14:21)*
Vrai ssi `i4 ≥ 5`. Les 5 sondes méthodologiques sont passées.

### `seal_6_imparfait((p list)) → bool`
**Sceau 6 — L'Imparfait** *(Apoc 13:18, le nombre 666)*
Vrai ssi `i5 < 6`. Le score d'imperfection (p-hacking, post-hoc, etc.) reste sous le seuil.

### `veritas_seal_7((p list) (fhash num)) → bool`
**Sceau 7 — Sabbat v2** *(Genèse 2:2-3)*
Vrai ssi `i6 = (sum i0..i5 + fhash) mod 7`. **Cross-link** : la proposition est scellée à la formule via fhash.
**Invariant** : `(= result (= (get p 6) (% (+ ... + fhash) 7)))`

---

## Helpers d'accès MTP

Extracteurs positionnels d'une formule MTP-v2.

### `expr_part_v2((f list)) → list`
Renvoie les `len-5` premiers tokens (l'expression RPN sans les 5 témoins).

### `witness_M_v2((f list)) → num`
5ᵉ token depuis la fin. Témoin M (somme opérandes mod 7).

### `witness_T_v2((f list)) → num`
4ᵉ token depuis la fin. Témoin T (nb opérateurs mod 7).

### `witness_P_7((f list)) → num`
3ᵉ token depuis la fin. Témoin V mod 7.

### `witness_P_11((f list)) → num`
2ᵉ token depuis la fin. Témoin V mod 11.

### `witness_P_13((f list)) → num`
Dernier token. Témoin V mod 13.

---

## Helpers de comptage

Reducers pour `reduce:list`.

### `add_if_operand((acc num) (tok num)) → num`
Reducer somme conditionnelle des opérandes (tokens < 1001).

### `sum_operands((expr list)) → num`
Somme des opérandes via `(reduce:list add_if_operand expr 0)`.

### `inc_if_operator((acc num) (tok num)) → num`
Reducer comptage conditionnel des opérateurs (tokens ≥ 1001).

### `count_operators((expr list)) → num`
Nombre d'opérateurs.

---

## Phases MTP

### `mane_I_v2((f list)) → bool`
**Phase MANE I**. Vérifie que `M` (témoin) = `sum_operands(expr) mod 7`.
**Invariant** : biconditionnel sur la prédiction de M.

### `mane_II_v2((f list)) → bool`
**Phase MANE II**. Vérifie que `T` = `count_operators(expr) mod 7`.

### `phares_v2((f list)) → bool`
**Phase PHARES v2**. Vérifie que :
- `V ≠ -999999` (calcul valide)
- `V mod 7 = P7`
- `V mod 11 = P11`
- `V mod 13 = P13`

### Helpers arithmétiques

- `safe_div(a, b)` : `floor(a / b)` ou `-999999` si `b = 0`
- `safe_mod(a, b)` : `a mod b` ou `-999999` si `b = 0`
- `apply_binary(op, a, b)` : dispatch des 8 opérateurs binaires + propagation sentinelle
- `apply_unary(op, a)` : dispatch des 2 opérateurs unaires + propagation sentinelle

---

## Évaluation RPN

### `tekel_step((stack list) (tok num)) → list`
Reducer une étape de la pile RPN :
- opérande → push
- opérateur → pop N, apply, push résultat
- underflow → sentinelle propagée

### `tekel_weigh((expr list)) → num`
Évaluation RPN complète : `get (reduce:list tekel_step expr (list)) 0`.
Retourne `V` ou `-999999` si erreur.

---

## Orchestration MTP

### `phases_passed_v2((f list)) → num`
Compteur séquentiel des phases passées (0..4). Cascade `if not mane_I_v2 → 0, ...`.
**Invariants** : `(>= result 0)`, `(<= result 4)`.

### `is_resolved_v2((f list)) → bool`
Prédicat global. Vrai ssi `phases_passed_v2 = 4`.

---

## VERITAS

### `formula_hash((f list)) → num`
Hash local de la formule MTP-v2 : `(M + T + P7 + P11 + P13) mod 7`. Engagement structurel utilisé pour le cross-link.

### `claim_formula((c list)) → list`
Extrait les `len-7` premiers tokens d'une claim — la partie formule.

### `claim_proposition((c list)) → list`
Extrait les 7 derniers tokens — la proposition de raisonnement.

### `veritas_open_seals((p list) (fhash num)) → num`
Compteur séquentiel des 7 sceaux d'une proposition Veritas, **avec sceau 7 cross-linké à fhash**. Retourne 0..7.

### `formula_attests((c list)) → bool`
Vrai ssi `is_resolved_v2(claim_formula(c))`. La formule passe ses 4 phases.

### `proposition_attests((c list)) → bool`
Vrai ssi `veritas_open_seals(claim_proposition(c), formula_hash(claim_formula(c))) = 7`. Le raisonnement est scellé pour cette formule spécifique.

### `attestation_level((c list)) → num`
Niveau combiné, 0..3 :
```
+ (if (formula_attests c) 1 0)
+ (if (proposition_attests c) 2 0)
```

### `veritas_verdict((c list)) → str`
Sortie textuelle finale :
- `"ATTESTEE"` si niveau = 3
- `"BRISEE A LA FORMULE"` si niveau = 2
- `"BRISEE AU RAISONNEMENT"` si niveau = 1
- `"BRISEE AUX DEUX"` si niveau = 0

---

## Récapitulatif des types

| Fonction | Signature |
|---|---|
| `seal_1_logos` … `seal_6_imparfait` | `(list) → bool` |
| `veritas_seal_7` | `(list, num) → bool` |
| `expr_part_v2`, `claim_formula`, `claim_proposition` | `(list) → list` |
| `witness_M_v2` … `witness_P_13` | `(list) → num` |
| `add_if_operand`, `inc_if_operator` | `(num, num) → num` |
| `sum_operands`, `count_operators`, `tekel_weigh` | `(list) → num` |
| `mane_I_v2`, `mane_II_v2`, `phares_v2`, `formula_attests`, `proposition_attests` | `(list) → bool` |
| `safe_div`, `safe_mod` | `(num, num) → num` |
| `apply_binary` | `(num, num, num) → num` |
| `apply_unary` | `(num, num) → num` |
| `tekel_step` | `(list, num) → list` |
| `phases_passed_v2`, `is_resolved_v2`, `attestation_level`, `formula_hash` | `(list) → num` |
| `veritas_open_seals` | `(list, num) → num` |
| `veritas_verdict` | `(list) → str` |
