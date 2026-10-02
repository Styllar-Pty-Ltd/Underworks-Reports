> **Template** for `ProjectProgress.md — in the project root, never docs/` — OCPF BC Agentic Development Framework, written at Full PRE-01; refreshed at every step start and exit gate. Keep every
> heading in this order; replace every `<placeholder>`; a section that doesn't apply keeps its heading
> with *N/A — reason* beneath it; work the closing checklist and leave it ticked; delete this note.
> The step file in `runbookSteps/` says what goes in each section; this template only fixes the shape.

# Project Progress — <Extension Name>

## Steps
| Phase | Step | Status |
|---|---|---|
| DEFINE | PRE-01 — State the Problem | In Progress |
| DEFINE | PRE-02 — Structured Gap Analysis | |
| DEFINE | 01 — Populate the Intake Sheet (Project Parameters) | |
| DESIGN | 02 — Craft the FRD | |
| DESIGN | 03 — Craft the TDD (+ Human Effort Estimate) | |
| DESIGN | 04 — Sanity Check and Validation | |
| BUILD | 05 — Plan the Code | |
| BUILD | 06 — Code Generation | |
| BUILD | 07 — Compile and Package, Troubleshoot, Iterate | |
| PROVE | 08 — Gap-Fit Test, Fidelity Validation | |
| PROVE | 09 — Code Review | |
| PROVE | 10 — Update Design Documents | |
| PROVE | 11 — Document the Code | |
| PROVE | 12 — Release to Users for Testing | |

## Usage and cost
Measured per step (ALL ALONG → Usage & Cost Tracking; Ops § Usage & Cost). Refreshed at every exit gate. Tokens from Claude Code's transcripts; Copilot steps show premium requests in *Notes*. Cost at published API rates.
| Step | Model(s) | Input | Output | Cache write | Cache read | Cost (USD) | Elapsed | Turns | Decisions | Sub-agent calls | Senior AL dev est. (h) | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| PRE-01 | | | | | | | | | | | | |
| **Total** | | | | | | | | | | | | |

*Prices: <source URL>, read <date>. Human estimate: `docs/HumanEffortEstimate.md`. Copilot steps: premium requests in Notes, `n/a` in the token and cost columns.*

## Before calling a refresh done
- [ ] The step table's `In Progress` row matches the open window in `.ocpf/usage.json`.
- [ ] Every step with a start timestamp has a usage row; a step that couldn't be measured says why.
- [ ] The Total row and the pricing footnote are current.

---
*Asking, in plain language, "Where are we in the process? What's next?" always gets a direct answer from this file (plus `docs/ProjectMemory.md` and `docs/ChangeLog.md` for the reasoning behind it), whether or not an agent session is running at that moment.*
