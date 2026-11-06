# latent-option-models-code

## Setup

Install [uv](https://docs.astral.sh/uv/), then create the environment from `uv.lock`:

```bash
uv sync
source .venv/bin/activate
```

This installs Python 3.13 and all dependencies, using CUDA 13.2 PyTorch wheels on Linux.

Run the tests with `uv run pytest`.
