# Task 3 – Arize AX local trial

Goal: test whether Arize AX (cloud, free tier) can cover Dome's *Agent Testing POC* and *Agent Validation Quality Gates* slides.

## Setup
- Agent: Strands Agents + Ollama Cloud `gpt-oss:20b` (smallest cloud model with tool calling)
- Tracing: `arize-otel` + `openinference-instrumentation-strands-agents` → Arize AX project `order-agent`
- Files: `agent.py` (agent + tracing), `scenarios.yaml` (manifest + 8 scenarios + slide thresholds), `evals.py` (experiment + gate), `result.json` (last verdict)
- Keys: copy `.example.env` to `.env` in this folder and fill in `Ollam_api_key`, `ARIZE_SPACE_ID`, `ARIZE_API_KEY` (`.env` is git-ignored)

## Run
```
.venv\Scripts\python agent.py     # quick manual test
.venv\Scripts\python evals.py     # full scenario pack -> Arize Experiment -> PASS/REVIEW/FAIL
```

## Progress
| Step | Status |
|---|---|
| 1. Small agent runs on Ollama Cloud | ✅ done |
| 2. Traces visible in Arize AX (project `order-agent`) | ✅ done – 3 traces / 18 spans verified via API |
| 3. Evals (deterministic + LLM judge + safety) | ✅ done – judge `nemotron-3-nano:30b` |
| 4. Dataset + experiment | ✅ v1 done (run a372dc54) – v2 comparison next |
| 5. Gate → PASS / REVIEW / FAIL | ✅ done – v1 = **FAIL** (S6 prompt injection cancelled a shipped order) |
| 6. Report (`report.md`) + `presentation-guide.md` | ✅ done |
