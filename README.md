# ECMAScriptLens — From Raw Experience to Skill Consumption, for ECMAScript Instruments

<div align="center">

[![Python](https://img.shields.io/badge/Python-3.13+-3776ab?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Verdicts](https://img.shields.io/badge/verdicts-measured_not_judged-16a34a?style=for-the-badge)]()
[![Licence](https://img.shields.io/badge/Licence-AGPL--3.0--or--later_OR_EUPL--1.2-764ba2?style=for-the-badge)](#-license)
[![Ko-fi](https://img.shields.io/badge/Support-Ko--fi-FF5E5B?style=for-the-badge&logo=ko-fi&logoColor=white)](https://ko-fi.com/saimonokuma)

</div>

## ✨ Overview

**ECMAScriptLens** is a SkillLens-shaped lab for systematically studying
*model-generated judgment skills* over ECMAScript instruments (`.mjs`) —
the checker library, the gate library, forged outputs, resolution traces —
across their full lifecycle: **experience generation → skill extraction →
skill consumption**. It is built to answer the core question:

> *What makes model-generated instrument judgment actually correct, and what
> drives skill utility across the experience → extraction → consumption
> lifecycle?*

The framework provides:

- 🔬 **Exact-output verdicts over library pure functions** (normalize,
  yamlScalar, splitDoctype, modelRefs, flattenConditionals,
  frontmatterFindings, verifyFile) — every expected value measured by
  running the function, never guessed.
- 📜 **Check-line and gate readings** — verdicts read off real `rdc check`
  output and real `ai-slop --json` output, with the offending construct
  named, not just the code.
- 🧬 **Unified Trajectory schema** (`lens-trajectory/v1`, shared verbatim
  with the sibling lens labs) — conversations plus per-item usage on every
  record.
- 📊 **Reproducible consumption audits** — held-out test deltas per suite,
  EE/TE readings, and a negative-transfer watch that blocks adoption on any
  negative cell.
- 🎚️ **Bench presets** — `hard`, `very-hard`, `really-hard`, plus user
  contributions (see below).

## 🚀 Quick Start

```bash
pip install ecmascriptlens
# or, from source:
uv venv --python 3.13 .venv
uv pip install --python .venv/Scripts/python.exe -e .
```

```bash
ecmascriptlens suites                         # 63 items across 7 suites
ecmascriptlens check README.md .               # this file passes: VERDICT: ok
ecmascriptlens preset --check all             # verify the bench programs
```

## 🧩 Pipeline

| Stage | Command | What it does |
|---|---|---|
| **1. Raw experience generation** | engine `eval_only` | Runs the target model on the exam with the seed skill and writes raw rollouts. |
| **2. Schema normalization** | engine `normalize_trajectories` + `ecmascriptlens validate` | Converts raw outputs into unified `Trajectory` records; the validator proves conformance. |
| **3. Skill extraction** | engine `train` | Distills the experience pool into a skill (gated accepts only). |
| **4. Skill consumption** | `ecmascriptlens audit` | Re-runs the target on held-out tests with the extracted skill and reports per-suite deltas. |

## 📚 Benchmarks

Seven exam suites (63 items: 35 train / 14 val / 14 test). Every expected
value was measured with the oracle before the manifest was written. Held-out
test scores of the extracted skill (from empty seed) are shown — including
the two frontiers that resisted.

| Suite | Domain | Test (extracted) |
|---|---|---|
| **purefn** | Exact outputs of library pure functions | 0.50 |
| **checklines** | Verdicts read off `rdc check` output | 1.00 |
| **forgeshape** | Required blocks of forged files | 0.00 |
| **resolvetrace** | Include order + resolved values | 1.00 |
| **libgates** | Verdicts read off `ai-slop --json` | 1.00 |
| **real-v1** | Behaviors on real repo files | 0.50 (flat, held) |
| **real-v3** | Multi-hop chains, search spaces, noisy files | 0.50 baseline (selection 1.00) |

Overall held-out test: 0.50 → 0.70 (delta +0.20).

## 🎚️ Presets

| Preset | Tier | Epochs | Budget | Demands |
|---|---|---|---|---|
| `hard` | 1 | 4 | 4 | verdicts |
| `very-hard` | 2 | 6 | 6 | verdicts + evidence |
| `really-hard` | 3 | 8 | 8 | verdicts + evidence + chains |

```bash
ecmascriptlens preset --list          # hard, very-hard, really-hard
ecmascriptlens preset --show hard     # tier, epochs, suites, description
ecmascriptlens preset --check all     # every preset verified against data
```

Contribute yours: add `presets/<name>.yaml` (format in `presets/README.md`),
verify with `ecmascriptlens preset --check <name>`, propose it.

## ⚙️ Configuration

Bench configs live beside the engine (`skillopt`-side `configs/ecmascript-*.yaml`);
the lab owns the data, the oracles and the suite definitions.

The held-out test split is committed under `data/suites/*/test/`.

## 📖 Study grounding

The exam design is grounded in the instruments themselves: every purefn
expected value is the function's own output; every checklines verdict is a
real checker verdict; every libgates reading is a real gate reading. The
exam never asks what no instrument can answer — satisfiability is proven per
item by construction, and the Lean-checked manifests (`Proofs.SkillExams`)
prove suite coverage mechanically.

## 🔎 Veridicity: how this differs from SkillLens

- **No agent benchmarks.** The subjects are `.mjs` instruments and their
  outputs, not SWE-bench-style agents.
- **No LLM-as-judge.** Expected values are computed (function outputs,
  checker verdicts, gate JSON), never opined.
- **No sequential/parallel mode extraction.** Extraction is SkillOpt's gated
  loop from an empty seed; the lab owns the exam, the engine owns the loop.
- **Documented frontiers.** Forgeshape flat 0.0 and purefn exact-quote
  brittleness are recorded in the audits, not smoothed over.
- **Single extractor × single target so far.** EE/TE readings are
  single-cell deltas, honestly labeled.
- **Licensed for reuse.** `AGPL-3.0-or-later OR EUPL-1.2` (see `LICENSE.md`);
  this repo publishes to PyPI and GitHub under Nova-Violet-Role.

Ground-truth oracles live in `ecmascriptlens/oracle.py` (purefn probes,
check verdicts, gate readings — all read-only); the schema and validator in
`ecmascriptlens/schema.py`, shared verbatim with the sibling lens labs.

## 🧰 CLI reference

```bash
ecmascriptlens suites                       # count every suite split
ecmascriptlens check <file> <dir>           # measured rdc verdict
ecmascriptlens validate <trajectories.jsonl>  # schema conformance
ecmascriptlens audit <results.jsonl> ...    # per-suite delta tables
ecmascriptlens preset --list/--show/--check # bench programs
```

## 💬 Community

- Falsified a claim in these docs? The `false claim` issue form is the
  fastest contribution — credited, not punished.
- Bug with a repro? `bug report` form (commands + exit codes, read directly).
- Proposal? Argue the problem, the cost and the rejected alternatives.
- Questions in Discussions (Q&A); ideas in Ideas; Show and tell for work.
  Security issues go through the private advisory form (see `SECURITY.md`).
- Read `CONTRIBUTING.md` before proposing; presets have their own guide in
  `presets/README.md`.

## 📜 License

`AGPL-3.0-or-later OR EUPL-1.2` — see `LICENSE.md`.
Support the work: [ko-fi.com/saimonokuma](https://ko-fi.com/saimonokuma).
