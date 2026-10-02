> **Template** for `docs/CodeReview.md` — OCPF BC Agentic Development Framework, written at Full Step 09. Keep every
> heading in this order; replace every `<placeholder>`; a section that doesn't apply keeps its heading
> with *N/A — reason* beneath it; work the closing checklist and leave it ticked; delete this note.
> The step file in `runbookSteps/` says what goes in each section; this template only fixes the shape.

# Code Review — <Extension Name>

**Date:** <date> · **Reviewer:** <reasoning role, model / github.com reviewer agent> · **Fixes applied by:** <main role / person>
**Reviewed:** every AL file at commit <sha> against `docs/TDD.md` v<n>, Standards Parts 1, 2, 4, 7, AL Guidelines *Best Practices* and *Vibe Coding Rules*, and the BCQuality snapshot <sha>.

## 1. Findings by dimension
Dimensions from `runbookSteps/09.md`. One row per finding; severity **Critical / Major / Minor / Note**.
| # | Dimension | File / object | Finding | Rule (Standards § / guideline) | Severity | Resolution | ChangeLog ref |
|---|---|---|---|---|---|---|---|

## 2. Dead-code and obsolete-reference scan
Standards §1.5 — 100 % of files.
| File | Result | Notes |
|---|---|---|

## 3. Fix batch presented for approval
All code-touching findings together, one decision (design-rule changes asked separately).
| Findings | Approved by | Date | Recompiled / repackaged / retested |
|---|---|---|---|

## 4. Pattern candidates
Findings that repeat across batches and look generalizable — flagged to the human for the OCPF BC AL Patterns Library, never added unilaterally.
| Pattern | Where it recurred | Flagged to |
|---|---|---|

## 5. Conflicts surfaced
Where the Standards Guide and AL Guidelines or BCQuality disagree — surfaced, not silently resolved.
| Conflict | Sources | Decision | Decided by |
|---|---|---|---|

## Before calling this done
- [ ] Every Critical and Major finding is resolved or explicitly accepted by name.
- [ ] §2 shows every file clean.
- [ ] Every code fix went through the Step 07 cycle; still 0 errors / 0 warnings.
