---
craft_id: chrona_ew_signal_classifier_v1
version: 0.2.0
project: chrona
license: Apache-2.0
tags: ['status:trained', 'project:chrona', 'provenance:synthetic_iq', 'release_metric:accuracy_vs_majority']
library_name: chrona-models
---

# chrona_ew_signal_classifier_v1

**Project:** chrona · **Version:** 0.2.0 · **License:** Apache-2.0
**Produced by:** [Intel — Universal ML Inference Engine](https://huntingtonai.com)

> Chrona EW Signal Classifier (modulation / emitter-type identification).

## Model description

`chrona_ew_signal_classifier_v1` is a **surrogate model** ("craft") used by the AHL Intel inference
engine to replace expensive numerical simulation with fast neural inference.
Output kind: **fields** (0 outputs). Chrona EW Signal Classifier (modulation / emitter-type identification).

- **Status:** trained
- **Type:** neural inference surrogate (physics-trained)
- **Parameters:** 469,164
- **Weights format:** `model.safetensors` (float32)
- **Device family:** `—`

## Intended use

Drop-in inference surrogate for chrona. You provide a physical **device
specification** (geometry, materials, doping, operating point); the package
returns the physical output channels below. The device→model featurization and
de-normalization are handled inside the package. Not intended for regimes
outside its training domain (below).

## Inputs & outputs

**Input — physical device specification** (`input.kind: device_spec`):

| Field | What you provide |
|---|---|
| `geometry` | rectilinear grid: per-axis coords in micrometers, or shape [Nx,Ny,Nz] |
| `regions` | [{type: NA|ND, material: Si|GaAs|InGaAs, concentration: <cm^-3>}] |
| `electrodes` | optional [{id, role: anode|cathode, bounds: [[i0,i1],[j0,j1],[k0,k1]]}] |
| `operating_point` | {applied_bias_v: <V>, bias_sweep_v?: [<V>...], temperature_k?: <K>} |

The internal encoding (how the device becomes model inputs) and the normalization are handled **inside the package** — you never build feature vectors by hand.

**Output — physical fields:**

| | |
|---|---|
| **Output channels** | 0 |
| **Parameters** | 469,164 |

## Training domain / compatibility

| Property | Value |
|---|---|
| Solution type | `—` |
| Physics models | — |
| Temperature | — K |
| Bias range (Vds) | — |
| Tags | `status:trained`, `project:chrona`, `provenance:synthetic_iq`, `release_metric:accuracy_vs_majority` |

## Training data

Trained on chrona simulation output for the `—` solution regime (physics models: see config). Labels derive from the numerical chrona engine / reference closed-forms; the data-generation pipeline is proprietary.

## Limitations & out-of-scope use

- **Do not** use outside the training domain above (solution type, temperature, bias range) — accuracy degrades with no guarantee.
- This is a **surrogate**, not the ground-truth solver: treat outputs as fast approximations; validate against the numerical engine where correctness is critical.
- Output kind is **fields** — consuming code must match the documented I/O contract exactly.


## Evaluation

| Metric | Value |
|---|---|
| **Held-out accuracy** | 0.8425 |

_**Held-out accuracy** is an independent benchmark (`offline_eval` on a held-out set); the loss rows are the model's own recorded train/val losses. See `config.json → metrics` for the machine-readable copy._

## Caveats & recommendations

- An independent **held-out** benchmark is reported (Held-out accuracy, via `offline_eval`); the loss rows remain the model's own recorded train/val losses.
- Verify integrity before use: `shasum -a 256 -c SHA256SUMS` (or `sha256sum -c SHA256SUMS` on Linux). Weights are `safetensors` — safe to load, no pickle.
- This is a **surrogate**, not the ground-truth solver — validate against the numerical engine where correctness is critical.

## Usage

Install the inference package and run it on a physical device spec — it fetches the
weights, builds the model, and returns physical fields (no PyTorch required):

```bash
pip install chrona-models
```

```python
from chrona_models import load

model = load("chrona_ew_signal_classifier_v1")           # pulls + verifies weights, builds the compiled pipeline
fields = model.predict(device_spec)  # physical device in -> physical fields out
```

`device_spec` is the physical device specification documented under **Inputs & outputs**
above. Featurization and de-normalization happen inside the package.

## Provenance

- **Author:** sim-train release (signal-exploitation, synthetic I/Q)
- **Trained:** 2026-07-18T21:49:19.226313+00:00
- **Source checksum (runtime .pt):** `979380e03cb03626f2c815f9181e123d8ddbad580a049a857647f4333925c2d3`

## Citation

```bibtex
@software{chrona_ew_signal_classifier_v1_0_2_0,
  title  = {chrona_ew_signal_classifier_v1},
  author = {AHL / Intel},
  year   = {2026},
  note   = {Version 0.2.0},
  url    = {https://downloads.chronarf.com/models/chrona_ew_signal_classifier_v1/0.2.0/}
}
```

## License

Licensed under the **Apache-2.0** (Apache-2.0) — free to use for
inference; redistribution and reverse engineering of the compiled inference core
are not permitted. See `LICENSE`.
