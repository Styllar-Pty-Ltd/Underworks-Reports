# BC App Build Routine — STEP 4 — Generate the Code

**Runbook version:** 3.1.0.0 · Lite edition · Phase: BUILD

> One step of the OCPF BC Agentic Development Framework runbook, fetched into `runbookSteps/` at the
> routine's first step and read **in full** the moment this step starts. The core runbook
> (`CLAUDE.md` or `.github/copilot-instructions.md`) carries only this step's stub; its Operating
> Rules and ALL ALONG sections apply throughout. **Standards §** is
> `standardsGuide/ocpfALDevStandardsGuide.md`; **Ops §** is `opsGuide/ocpfOperationsGuide.md`.

---


**Inputs:** `docs/DesignDoc.md`, `docs/ProjectParameters.md`, symbol file, batch plan, pre-flight checklist.

**Actions — per object, in order:**
1. **Approval came at Step 3** — don't ask again per object. With two batches, pause before the
   second only if the human chose that. Stop and ask on any pre-flight failure the Design Doc
   can't resolve, or any deviation from `docs/DesignDoc.md`.
2. Extract source-table and field data for this object from the symbol file.
3. Run the pre-generation pre-flight pass; fix the Design Doc before generating if anything fails.
4. Generate the AL file **from the standard template in Standards §1.3**, substituting only Step 1
   parameter values: one `namespace` (omitted entirely if `Use Namespace` = `No`), one `using`
   (from the symbol file), `ODataKeyFields = SystemId`, exactly one of `DelayedInsert = true` /
   `Editable = false`, `Caption` + `ToolTip` + `ApplicationArea = All` on every field. No dead
   code, no empty triggers, no commented-out fields, no `// TODO` (**Standards §1.1–§1.5**).
   Write `Caption` and `ToolTip` as self-describing schema for API consumers, not UI filler — they
   flow into OData `$metadata` and are what a developer or AI agent reads when discovering the
   endpoint (**Standards §2.5–§2.6**). Single-language label syntax only — never `CaptionML`,
   `ToolTipML`, any other ML property, or `TextConst` (**Standards §1.7**). Messages as `Label`s
   (**§8.3**), W1 source wording from the glossary, API caption locking per Step 2 (**§8.6**).
   **Source text only** — no translation file is touched until Step 5.
   **Client pages have their own rules (Standards Part 11), applied while writing:** every child
   list or list part gets the six elements of **§11.1** (property link, hidden link control,
   two-group `OnNewRecord` read, `TestField` in the table's `OnInsert`, `DelayedInsert = true`, no
   `Init()` after seeding); every number-series field gets `TableRelation = "No. Series"` on the
   setup table with the wizard bound to a temporary copy of it, and numbering uses codeunit
   `"No. Series"` (**§11.2**). Read the matching `patterns/` files first (Step 3).
5. Run the post-generation pre-flight pass immediately on this file — including the §11.1 and
   §11.2 checks for any child list, list part, setup page, or wizard. **Do not invoke the AL
   compiler** (Operating Rule 4).
6. Don't move to the next object until this one's pre-flight, including symbol verification, is
   clean.
- **Before moving past this step, verify permission-set coverage explicitly** across every table
  generated (**Standards §5.3**; vacuously satisfied if the extension owns no tables).

**Outputs:** Every AL file, lint-clean including symbol verification; ChangeLog entries for any
deviation from `docs/DesignDoc.md`. The extension is **not** compiled yet.

**Exit gate:** Every planned object generated and pre-flight-clean; permission-set coverage
verified. A clean compile is not required to close this gate — Step 4 hands off directly into
Step 5's mandatory compile-and-package.
