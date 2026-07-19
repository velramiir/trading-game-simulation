# trading-game-simulation

Pre-prototype economy simulation for balance testing.

## Setup

Requires [uv](https://docs.astral.sh/uv/).

```bash
uv sync            # create venv + install dev deps
uv run trading-sim # (once an entry point is added) or:
uv run python -m trading_sim.run
```

## Develop

```bash
uv run pytest      # run tests
uv run ruff check  # lint
uv run ruff format # format
```