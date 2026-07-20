# chrona-models

Runnable neural-surrogate inference for the **Chrona** RF / signal-simulation platform.
Torch-free (ONNX + `onnxruntime`, CPU).

```python
from chrona_models import load

model = load("chrona_ew_signal_classifier_v1")
out = model.predict(inputs={"iq": iq})   # iq: [B, 2, 1024] (I/Q samples)
distribution = out["distribution"]        # [B, 8] class probabilities (softmax)
```

`model.input_names` / `model.output_names` and each craft's `MODEL_CARD.md` document I/O.

## Install
```
pip install chrona-models      # numpy + onnxruntime only, no torch
```

## Scope
- **Preview build**: ships `chrona_ew_signal_classifier_v1`; more crafts follow.
- **Fast surrogates, not the ground-truth solver.**

## License
Closed-source, compiled distribution under the **AHL Model EULA** (see `LICENSE`).
© 2025–2026 Huntington Applied.
