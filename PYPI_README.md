# ECMAScriptLens — harness usage

Deterministic oracles + benchmark suites for ECMAScript judgment
(check verdicts, build drift, forge shape, resolve bytes, lib gates).
No model required to run the harness — every verdict is computed,
byte-exact, on your machine.

## Requirements

- Python 3.11–3.13 (`3.13` recommended; `3.14` is not supported)
- `PyYAML >= 6` (only dependency)
- Node.js (only to run the *reference* checkers the oracles mirror;
  the oracles themselves are pure Python)
- No GPU. No network. No model key for oracle use.

## Install

```bash
pip install ecmascriptlens
# or: uv pip install ecmascriptlens
```

## Commands

All five verbs are read-only and offline:

```bash
ecmascriptlens suites                    # list suites + item counts
ecmascriptlens check <file> <dir>        # verdict over one file
ecmascriptlens validate                  # validate every bundled manifest
ecmascriptlens audit <results...>        # score rollout results vs manifests
ecmascriptlens preset --list             # list bench presets
ecmascriptlens preset --show hard        # show a preset (tier/epochs/suites)
ecmascriptlens preset --check all        # verify presets against the data
```

(`python -m ecmascriptlens.cli` works identically.)

## What each command does

- `suites` — inventory: suite names with train/val/test counts
  (purefn, checklines, forgeshape, resolvetrace, libgates, real-v1,
  real-v3).
- `check` — exact-output verdicts: quoting, findings, gate readings
  over one file. Exit 0 with the verdict plus per-assertion detail.
- `validate` — schema-checks every bundled `items.json`.
- `audit` — scores model rollouts against manifests: hard, soft,
  per-tier. The same metric the published deltas were measured with.
- `preset` — the bench programs: `hard` (tier 1), `very-hard` (tier 2),
  `really-hard` (tier 3).

## Examples

```bash
ecmascriptlens check README.md .
# VERDICT: ok

ecmascriptlens preset --check all
# ok   hard
# ok   really-hard
# ok   very-hard
```

## Contribute a preset

Add `presets/<name>.yaml` (see `presets/README.md` for the format),
then `ecmascriptlens preset --check <name>` before proposing it.

## Capabilities and limits

- Judges ECMAScript-shaped behavior: pure-function outputs, checker
  verdicts, forge shapes, resolve orders, library gate readings.
- Does not train models, does not call networks, does not write files.
- Verdicts are deterministic: same bytes in, same verdict out.

## License and support

`AGPL-3.0-or-later OR EUPL-1.2` (see `LICENSE.md`).
Support the work: [ko-fi.com/saimonokuma](https://ko-fi.com/saimonokuma).
Falsified a claim in these docs? File it — the fastest contribution:
`false claim` issue form.
