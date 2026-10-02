> **Template** for `docs/HumanEffortEstimate.md` — OCPF BC Agentic Development Framework, written at Full Step 03 / Lite Step 2, in the same pass as the design. Keep every
> heading in this order; replace every `<placeholder>`; a section that doesn't apply keeps its heading
> with *N/A — reason* beneath it; work the closing checklist and leave it ticked; delete this note.
> The step file in `runbookSteps/` says what goes in each section; this template only fixes the shape.

# Human Effort Estimate — <Extension Name>

**Date:** <date> · **Estimated by:** <drafting role, model> · **Basis:** `docs/TDD.md` v<n> (Full) or `docs/DesignDoc.md` v<n> (Lite) · **Reviewed by:** <name>

> **What this is.** The billable hours an **experienced senior AL developer** (5+ years of Business
> Central extension work, fluent in AL, the AL tools, and the standards this project applies) would
> need to deliver the same scope by hand, working from the same requirements. It is an **estimate**
> for reading next to the measured AI cost in the usage table (`ProjectProgress.md` / `docs/UsageReport.md`),
> not a quote, not a timesheet, and not a claim about any particular developer. Every assumption is
> in §1 so the reader can change it.

## 1. Assumptions
| Assumption | Value used | Change it if |
|---|---|---|
| Developer profile | Senior AL developer, works alone, no ramp-up on BC | a team, or a developer new to BC |
| Working day | 8 billable hours | |
| Requirements | As complete as `docs/FRD.md` / `docs/DesignDoc.md`; no re-discovery | the human would also gather requirements |
| Testing | Manual sandbox testing by the developer, plus the unit tests named below | a separate QA function |
| Meetings, reviews, sign-offs | Included as *Review and sign-off* | |
| Baselines | §2's framework baselines (mid-point unless complexity says otherwise) | your own rate card or history |

## 2. Baselines used
Framework defaults for a senior AL developer, per unit. Pick the low end for simple and the high end for complex; write the reason in §3.
| Task | Unit | Baseline (h) |
|---|---|---|
| Requirements write-up (FRD-equivalent) | per 10 entities | 4 – 8 |
| Technical design (TDD-equivalent, incl. ID allocation, field-by-field decisions) | per 10 objects | 6 – 12 |
| New table | each | 2 – 4 |
| Table extension | each | 1 – 2 |
| List / card page | each | 2 – 4 |
| Page extension | each | 1 – 2 |
| API page or API query | each | 1.5 – 3 |
| Codeunit — helpers, subscribers, simple validation | each | 2 – 4 |
| Codeunit — business logic (posting, no. series, complex validation) | each | 8 – 24 |
| Report with layout | each | 6 – 16 |
| Enum / enum extension | each | 0.5 |
| Permission set (VIEW + EDIT pair) | per pair | 1 – 2 |
| Upgrade codeunit | each | 3 – 6 |
| Event publisher / subscriber wiring | each | 1 – 2 |
| Assisted setup wizard / role-center cues / Departments placement | each feature | 4 – 8 |
| Translation drafting | per language, per 20 labels | 0.5 (+ reviewer time, not counted) |
| Compile / package / deploy troubleshooting | share of build hours | 10 – 20 % |
| Automated tests (test codeunits) | share of build hours | 30 – 50 % |
| Code review and fixes | share of build hours | 10 – 15 % |
| Documentation, user guide, test script, deployment guide | per project | 4 – 12 |
| Release testing support and fixes | per project | 2 – 6 |

## 3. Estimate by task
One row per task; objects grouped by type where the same reasoning applies.
| # | Task | Object(s) / scope | Complexity and why | Hours | Framework step |
|---|---|---|---|---|---|
| 1 | Requirements | | | | 02 / Lite 2 |
| 2 | Technical design | | | | 03 / Lite 2 |
| 3 | <object type> — <IDs> | | | | 06 / Lite 4 |
| … | | | | | |
| n | Documentation set | | | | 11 / Lite 6 |

## 4. Totals
| Phase | Hours | Days (8 h) |
|---|---|---|
| DEFINE + DESIGN | | |
| BUILD | | |
| PROVE | | |
| **Total** | | |

## 5. What would move this estimate
- <e.g., the ID range Microsoft assigns for AppSource, an unresolved open question, a localization change>

## Before calling this done
- [ ] Every object in the Object Register appears in §3, alone or in a group.
- [ ] Every row's hours can be traced to a §2 baseline and a stated complexity reason.
- [ ] §1 lists every assumption a reader would need to change to reuse this estimate.
- [ ] The step column lets the usage table sum these hours per framework step.
