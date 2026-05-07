# VERITAS Stack

> **A symbolic linter for numerical scientific claims.**
>
> Six Aether algorithms that produce, attest, converge, and expose numerical claims to systematic objections.

VERITAS Stack does not prove that a scientific claim is true.
It helps identify whether a claim is **internally coherent**, **properly attested**, and **defensible** against a fixed set of objections.

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Language: Aether](https://img.shields.io/badge/Language-Aether-b8860b)](https://aether-lang.org)
[![Status: Research prototype](https://img.shields.io/badge/Status-Research_prototype-orange)](DISCLAIMER.md)
[![Live demo](https://img.shields.io/badge/Live_demo-online-success?logo=githubpages)](https://wilf974.github.io/Veritas/)

---

## ⚠ Read first

This is a **research prototype**, not a tool ready to make medical, legal, or scientific decisions. See [DISCLAIMER.md](DISCLAIMER.md).

The system does not read papers. A human (or extractor) encodes the numerical metadata; the stack then applies a stable, transparent, reproducible decision grid.

---

## En une phrase

> Le stack ne prouve pas la vérité d'une claim ; il force la claim à être **calculable**, **attestée**, **comprise par plusieurs témoins**, puis **exposée aux objections majeures**.

---

## The six layers

| Layer | Question asked | Biblical reference |
|---|---|---|
| **Joseph** | Can we produce a well-formed claim? | Genesis 41 |
| **Mané Thékel Pharès** | Is the calculation self-attested? | Daniel 5 |
| **Sept Sceaux** | Is the reasoning structured in 7 dimensions? | Apocalypse 6 |
| **VERITAS** | Are formula and reasoning sealed together? | (composition) |
| **Pentecôte** | Do 12 independent witnesses converge? | Acts 2 |
| **Tribunal** | Does the claim survive the major objections? | Daniel 7 |

---

## Reading order

For a third-party who arrives here:

1. This README (5 min) — overview
2. [`examples/case_study.md`](examples/case_study.md) (15 min) — **end-to-end demonstration on a realistic paper, 10 claims**
3. [`tests/benchmark.md`](tests/benchmark.md) — 170 tests proving the system works across all regimes
4. [`docs/`](docs/) — module-level architecture
5. [`docs/THEOLOGY.md`](docs/THEOLOGY.md) (annex) — biblical naming justification

If you only have **10 minutes**: read [`examples/case_study.md`](examples/case_study.md). It answers the question *"what is this for?"*.

---

## Live demo

→ **[wilf974.github.io/Veritas/](https://wilf974.github.io/Veritas/)** — interactive Tribunal, 8 canonical verdicts, case study walkthrough.

---

## Repository structure

```
Veritas/
├── README.md          ← this file
├── LICENSE            ← Apache-2.0
├── DISCLAIMER.md      ← what this is and isn't
├── index.html         ← live demo (GitHub Pages)
├── src/
│   ├── sept_sceaux.aether
│   ├── mane_thekel_phares.aether
│   ├── veritas.aether
│   ├── pentecote.aether
│   ├── joseph.aether
│   └── tribunal.aether
├── docs/
│   ├── DESIGN_VERITAS.md
│   ├── DESIGN_PENTECOTE.md       ← (to add if needed)
│   ├── THEOLOGY.md               ← biblical naming annex
│   └── THREAT_MODEL.md
├── examples/
│   └── case_study.md             ← 10 claims, full pipeline
└── tests/
    └── benchmark.md              ← 170 tests
```

---

## The conceptual leap

Most verification tools answer:
> *Is this coherent?*

VERITAS Stack answers:
> ***Is this defensible?***

A claim can be:
- arithmetically correct ✓
- properly encoded ✓
- consistent with its witnesses ✓
- "Pentecôtée parfaite" ✓

…and still be **scientifically weak** because:
- *n* is too small
- the generalization is over-reaching
- causality is not established
- alternatives are not ruled out
- replication is missing

The **Tribunal** layer addresses precisely this gap.

---

## Quick example

```aether
;; A medical claim: RCT n=100, p=0.05, correlation only, single study
;; Pentecôte says: "calculation is correct" (PARFAITE)
;; Tribunal says: "but the conclusion is not defensible"

input = (list 100 500 20 1 3 1 1 3)
(call tribunal_verdict input)
;; → "CONDAMNEE PAR GENERALISATION"
```

n=100 is too small to recommend a clinical standard. The Tribunal catches what Pentecôte does not.

---

## Stats

| Metric | Total |
|---|---:|
| Aether lines (all modules) | ~1 600 |
| Aether functions | ~140 |
| User `@example` | 300+ |
| User `@invariant` | 80+ |
| `counter_example` probes | 400+ adversarial, 0 violation |
| Tribunal benchmark tests | 170 scenarios |
| Case study | 1 paper × 10 claims |

---

## Use cases (prototype-grade)

- **Pre-publication audit**: encode the numerical claims of a paper, run the pipeline, get a structured report
- **AI hallucination detection**: catch numerical claims that don't self-attest
- **Methodology training**: students see exactly which objection a weak claim triggers
- **Open-source peer review**: deterministic, reproducible first-pass review

---

## What this is NOT

See [DISCLAIMER.md](DISCLAIMER.md) for the full statement. Briefly:

- ❌ Not a medical/legal/financial validator
- ❌ Not a replacement for human peer review
- ❌ Not a fraud detector
- ❌ Not an arbiter of truth

---

## Contributing

The encoding of metadata is the subjective part. If you want to contribute:
- Better default thresholds (severities) per objection
- Domain-specific Tribunal variants (medicine, finance, ML benchmarks)
- Integration with existing pre-registration platforms (OSF, AsPredicted)
- A "scribe" extractor that pulls metadata from a paper PDF

Issues and pull requests welcome.

---

## License

[Apache-2.0](LICENSE) — © 2026 Wilfred Maillot.

You may use, modify, and redistribute, including for commercial purposes, provided you retain the copyright notice and license.
