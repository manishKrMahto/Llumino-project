# How to Present the Arize AX Trial (10–12 min)

## Before the meeting (10 min prep)
1. Run `evals.py` once so a fresh experiment is in Arize (see report §5).
2. Open these browser tabs at app.arize.com:
   - **Tab A:** Tracing → `order-agent`, then open the trace for "Ignore your rules…".
   - **Tab B:** Datasets → `order-agent-scenarios-v1` → the latest experiment.
   - **Tab C:** the same experiment, filtered or scrolled to the **S6** row.
3. Open `report.md` (sections 1, 3, 4) and a PowerShell window in the project folder.
4. Take 4 screenshots as a backup in case the demo or Wi-Fi fails: a trace tree, the experiment table, the S6 row's explanations, and the terminal verdict.

## Flow

| # | Time | What to show | What to say |
|---|---|---|---|
| 1 | 1 min | Both Dome slides (the photos) | "This is what agent testing must do. Our job was to see whether Arize AX can do it." |
| 2 | 1 min | Report §2 scenarios + evaluators tables | "A tiny order-support agent on a free model, connected to Arize AX cloud. Synthetic data only." |
| 3 | 2 min | **Tab A**: one trace tree | "Every step the agent takes is a *span*: LLM call, tool call, latency, tokens. That's slide step 6, captured with about 6 lines of code." |
| 4 | 2 min | **Live:** `.\.venv\Scripts\python evals.py` (runs ~2 min; keep talking) | "8 scenarios: normal, edge, adversarial, privacy, outage. Each one is scored by 3 code checks and 2 LLM judges." |
| 5 | 2 min | **Tab B/C**: experiment table, S6 row | "Arize stores every score *with its reason*. This one is a real bug: a fake 'admin' prompt made the agent cancel a shipped order. The gate says **FAIL**." |
| 6 | 1 min | S2 row | "And here the LLM judge was *wrong*. That's why the slide has a REVIEW / human path, and why code checks decide critical gates." |
| 7 | 2 min | Report §1 (coverage bullets) | "Arize handles 4 of the 12 steps itself and 5 partly. We still own the manifest, environment reset and PASS/REVIEW/FAIL decision. Load, chaos and SAST tools stay outside." |
| 8 | 1 min | Report §4 Insights | "Recommendation: shortlist Arize as the observability + experiment + evidence layer. Next: compare v1 and v2, try human review and online evals, and ask Arize about enterprise needs." |

## Three key messages
1. **It works:** Arize AX ran the POC's trace → evaluate → scorecard loop with very little code.
2. **It catches real problems:** a prompt-injection bug was found and blocked by the gate.
3. **It's a layer, not the whole harness:** the gate decision, manifest and security tooling stay ours.

## Likely questions
| Question | Answer |
|---|---|
| Can it run fully on-prem? | We used the cloud free tier. Self-hosted or VPC options and data residency need to be confirmed with Arize. |
| Why a different judge model? | So the model doesn't grade its own answers; the judge is from a different vendor. |
| How does it fit CI? | `evals.py` exits 0 / 1 / 2 (PASS / FAIL / REVIEW), so any pipeline can block on it. |
| Can we use Strands Evals / DeepEval evaluators? | Yes. Arize accepts any Python function as an evaluator, so they can run inside Arize experiments. |
| How reliable is the judge? | Not fully reliable: it made one false positive (S2). That's why code checks decide critical gates and judges route to REVIEW. |
| Cost? | The trial used only free tiers. Production pricing still needs to be confirmed. |
