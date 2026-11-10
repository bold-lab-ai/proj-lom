# latent-option-models-code

## Setup

Install [uv](https://docs.astral.sh/uv/), then run everything through `uv run`. It creates the environment from `uv.lock` on first use and keeps it in sync, so there is no environment to activate:

```bash
uv run scripts/pretrain.py --method lom --signal latent \
    --config experiments/benchmark/config.yaml
```

This installs Python 3.13 and all dependencies, using CUDA 12.6 PyTorch wheels on Linux, which need an NVIDIA driver for CUDA 12.6 or newer.

Run the tests with `uv run pytest`.
