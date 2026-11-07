# latent-option-models-code

## Setup

Install [uv](https://docs.astral.sh/uv/), then create the environment from `uv.lock`:

```bash
uv sync
source .venv/bin/activate
```

This installs Python 3.13 and all dependencies, using CUDA 12.6 PyTorch wheels on Linux, which need an NVIDIA driver for CUDA 12.6 or newer.

Run the tests with `uv run pytest`.
