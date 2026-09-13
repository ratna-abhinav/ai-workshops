# Agentic AI Workshop

This course has **Day 0 pre-work plus seven instructional days**. Complete Day 0 before the first live class.

You will begin with one model call and gradually build stateful, observable, tool-using, multi-agent, and durable AI systems. Every tutorial uses the same [0/1-knapsack debugging example](RUNNING_EXAMPLE.md), so you can focus on the new agent concept instead of learning a new problem each day.

## Start here

Use these instructions from the workshop root—the folder containing this README.

### 1. Check Python and uv

Verify that `uv` and a supported Python version (Python 3.12, 3.13, or 3.14) are installed:

```bash
uv --version
python --version
```

If `uv` is not installed, install it using the official installer:
- **macOS/Linux**: `curl -LsSf https://astral.sh/uv/install.sh | sh`
- **Windows (PowerShell)**: `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`

> [!TIP]
> `uv` can also manage and download Python versions directly. For example, to install and use Python 3.14:
> ```bash
> uv python install 3.14
> ```

### 2. Sync the workshop environment with uv

This project is managed with `uv`: dependencies are declared in
`pyproject.toml` and pinned in `uv.lock`. `uv` reads `.python-version`
(Python 3.14) and creates/updates `.venv` for you — do not create the
virtual environment or install packages manually.

```bash
# From the workshop root (the folder containing pyproject.toml):
uv sync
```

To add a dependency later, use `uv add <package>` (which updates
`pyproject.toml` and `uv.lock`), never edit `uv.lock` by hand.

Register the environment as a Jupyter kernel:

```bash
uv run python -m ipykernel install --user \
  --name agentic-ai-workshop \
  --display-name "Python (Agentic AI Workshop)"
```

### 3. Confirm the workshop model is running

The model service at `http://127.0.0.1:8317/v1` is separate from this repository. Your instructor or workshop host starts it. Ask the instructor to confirm it is available before configuring MLflow.

The value `123456` used below is the local workshop credential, not a personal provider secret.

### 4. Start MLflow

```bash
./workshop start
```

Open [http://127.0.0.1:5001](http://127.0.0.1:5001). Keep MLflow running while working through Days 0–6.

### 5. Configure the MLflow Gateway once

In MLflow:

1. Open **Settings → LLM Connections**.
2. Create a connection named `local-gemini-key`.
3. Select **OpenAI** because the workshop model implements the OpenAI-compatible API.
4. Enter `123456` as the key.
5. Set the base URL to `http://127.0.0.1:8317/v1` and save.
6. Open **AI Gateway → Endpoints**.
7. Create an endpoint named `workshop-gemini`.
8. Select the OpenAI provider, model `gemini-3-flash`, and connection `local-gemini-key`.
9. Enable usage tracking and save.

### 6. Verify everything

```bash
./workshop doctor
```

Core checks should show `✓`. Judge0 may show a warning; it is not required until Day 4, and Days 4–5 tutorials provide a mock mode. If a check fails, fix the first failure and run the command again.

## Begin Day 0

```bash
cd day-0/tutorial
uv run jupyter lab
```

Open `day-0.ipynb`, choose **Python (Agentic AI Workshop)**, and run one cell at a time from top to bottom.

## Course map

| Session | Topic | Start page |
| --- | --- | --- |
| Day 0 pre-work | Setup and one-state AI | [Day 0](day-0/README.md) |
| Day 1 | Bounded retry and ReAct | [Day 1](day-1/README.md) |
| Day 2 | Multi-state graphs and routing | [Day 2](day-2/README.md) |
| Day 3 | Traces, datasets, and evaluation | [Day 3](day-3/README.md) |
| Day 4 | Guarded execution tools | [Day 4](day-4/README.md) |
| Day 5 | Multi-agent stress testing | [Day 5](day-5/README.md) |
| Day 6 | Benchmarking and LLM-as-judge | [Day 6](day-6/README.md) |
| Day 7 | Production-shaped full stack | [Day 7](day-7/README.md) |

Every day has:

- `tutorial/` — completed material used in the recorded lesson.
- `exercise/` — checkpoint-based homework. Do not use **Run All** until every TODO is complete.

## Judge0 from Day 4

If the root `.env` file does not exist, create it from the template:

```bash
cp .env.example .env
```

Leave `EXECUTION_MODE=mock` for the Day 4–5 tutorials. The mock works only for the documented knapsack example and is not proof that arbitrary code executed. Original homework programs require `EXECUTION_MODE=judge0` plus a Judge0 provider key.

## Docker on Day 7

Day 7 requires Docker Desktop. Its Compose stack includes its own MLflow server, so stop the shared server before starting Day 7:

```bash
./workshop stop
```

Then follow the [Day 7 start page](day-7/README.md).

## If you get stuck

Run `./workshop doctor`, then open [TROUBLESHOOTING.md](TROUBLESHOOTING.md). Definitions for unfamiliar terms are in [GLOSSARY.md](GLOSSARY.md).

When you finish Days 0–6, stop MLflow:

```bash
./workshop stop
```

## For instructors

`QA/` contains executed reference solutions and must not be distributed to learners. Create a clean learner copy with `./scripts/make-student-release.sh`.

After editing the course, run:

```bash
uv run python scripts/check_course.py
uv run python QA/check_qa.py
```
