# latent-option-models-code

## Setup

Install [uv](https://docs.astral.sh/uv/), then run everything through `uv run`. It creates the environment from `uv.lock` on first use and keeps it in sync, so there is no environment to activate:

```bash
uv run scripts/pretrain.py --method lom --signal latent \
    --config experiments/benchmark/config.yaml
```

This installs Python 3.13 and all dependencies, using CUDA 12.6 PyTorch wheels on Linux, which need an NVIDIA driver for CUDA 12.6 or newer.

Run the tests with `uv run pytest`.

On macOS, PyTorch provides wheels only for Apple Silicon on macOS 14 or newer. NLE has no macOS wheels and builds from source on first sync, which needs the Xcode Command Line Tools (`xcode-select --install`).

## Contributing

GitHub doesn't enforce these rules for this repo, so follow them by hand. `.claude/skills/git-workflow/SKILL.md` has the full version, which Claude Code agents load automatically.

- **Branches:** start from the latest `origin/main` and name them `<type>/<short-description>`, e.g. `fix/benchmark-done-marker`.
- **Commits:** use [Conventional Commits](https://www.conventionalcommits.org/) (`<type>(<scope>): <summary>`) with one logical change per commit. In the commit body, say why the change is needed, what was tested and where, and what wasn't tested.
- **Pull requests:** use a Conventional Commits title, and include a description with Summary, Changes, Testing and Not tested sections.
- **Merging:** use **Rebase and merge** only, never squash or merge commits, so each commit lands on `main` as written. To update a branch, rebase it onto `main` and push with `--force-with-lease`, never plain `--force`.
- **After merging:** delete the branch on GitHub and locally (`gh pr merge <number> --rebase --delete-branch`).
