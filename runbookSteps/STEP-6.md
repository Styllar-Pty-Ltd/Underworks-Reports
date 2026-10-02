# BC App Build Routine — STEP 6 — Review, Gap-Check & Finalize Docs

**Runbook version:** 3.1.0.0 · Lite edition · Phase: PROVE

> One step of the OCPF BC Agentic Development Framework runbook, fetched into `runbookSteps/` at the
> routine's first step and read **in full** the moment this step starts. The core runbook
> (`CLAUDE.md` or `.github/copilot-instructions.md`) carries only this step's stub; its Operating
> Rules and ALL ALONG sections apply throughout. **Standards §** is
> `standardsGuide/ocpfALDevStandardsGuide.md`; **Ops §** is `opsGuide/ocpfOperationsGuide.md`.

---


**Inputs:** The built extension, `docs/DesignDoc.md`, `docs/ChangeLog.md`, `documentTemplates/Docs.md`, `documentTemplates/TestScript.md` (each document is written from its template, Ops § Documents).

**Actions:**
- **Gap-check** — compare `docs/DesignDoc.md` against the as-built code. For each divergence: object
  planned but not built (intentional or oversight?), object built but not planned (scope creep or
  gap-fill?), a rule implemented differently (is there a ChangeLog entry?). Classify each as
  **Intentional**, **Oversight** (fix now via the Step 5 cycle), or **Spec stale** (code's right —
  update `docs/DesignDoc.md`).
- **Code review** — one pass across every object: consistent structure/naming/formatting
  throughout (small projects still drift between the first file written and the last); no dead
  code (**Standards §1.5**); no reference to anything with `ObsoleteState = Pending`/`Removed`,
  unconditionally and with no version check (**Standards §3.2–§3.3**); both permission sets named
  with the App Code (**§5.4**). `Rec.`-prefix everywhere and every table's `tabledata` coverage are
  already proven by the last 0/0 compile — confirm it ran with the analyzers this framework
  requires (ALL ALONG → Analyzers) and nothing is suppressed, rather than re-deriving either check
  by hand. Correct `DelayedInsert`/`Editable` per data mutability (**§2.2**) still needs a human
  read; a compiler can't judge it. **Then run the full
  Anti-Patterns table — Standards Part 7 — against the codebase.** Then check every child list or
  list part against all six parts of **§11.1** and every number-series field and numbered table
  against **§11.2** — read each `OnNewRecord`, each child table's `OnInsert`, and each wizard's
  field bindings, not just the page properties. The Part 7 table is one table and it reads in
  a couple of minutes; it's the single highest-value thing the Standards Guide gives a Lite
  project, because most of what it catches is invisible until publish or until a consumer hits
  it. `TranslationFile` is on for every project (**§8.2**), so a 0/0 compile already proves the
  compiled codebase is free of `CaptionML`, `ToolTipML`, and the rest via `AL0424` — search anyway
  for anything added since that last compile, human-pasted included, so nothing new slips past
  before the next one. Read the code against
  AL Guidelines' *Best Practices* and *Vibe Coding Rules* for anything the Standards Guide doesn't
  already cover (the Standards Guide wins on any conflict — surface it rather than picking a side
  silently). **Translations:** run every technical translation check with all rules enabled, then
  review against **Standards Part 8** — no hard-coded user-facing strings, `Comment`s on
  placeholders, glossary terms used consistently, likely truncation in longer languages, and API
  caption locking re-verified against the Step 2 records. Then invoke the BCQuality snapshot's `skills/entry.md` dispatch flow (fetched at Step 3) as
  an additional, independent pass, and fold its findings in the same way as your own — never
  applied blind. If the fetch didn't happen or the snapshot is missing, say so rather than
  silently skipping this pass.
- **Update `docs/DesignDoc.md` in place** to reflect the as-built reality — final object inventory, any
  naming or exception that emerged during BUILD, a short deviation summary pointing at the
  relevant `docs/ChangeLog.md` entries. There's no separate as-built document in Lite; one file, kept
  current, is the point.
- Present this step's fixes together for one approval, as in Step 5. Any fix this step produces
  follows the Step 5 cycle (recompile, repackage, redeploy, retest) before this step closes; a
  comment/formatting-only fix doesn't need a fresh package.
- **Write `docs/Docs.md`** — one combined reference covering everything the full framework splits
  across four documents:
  - *API/dev reference*, generated from the actual code, not memory: one section per object, one
    row per field (identifier, source name, description, R/W status); a quick-start (auth, one
    request, one response); `$filter`/`$select` examples; create/update/delete examples; known
    limitations.
  - A Mermaid `erDiagram` of the schema, generated from the actual objects — every table the
    extension owns *and* every standard table it touches via `TableRelation`/`tableextension`.
    **Render it before shipping it** (e.g. `npx @mermaid-js/mermaid-cli`) — a syntactically
    invalid diagram looks fine in the source and only fails wherever it's finally viewed.
  - A short **user guide** section: what the feature is for, how to do each task, what to do when
    something is refused — written for the person clicking around in BC, not a developer.
  - A short **deployment** section: version requirements, install procedure, which permission sets
    map to which roles, uninstall, and which users to reassign if a release renames a permission set.
- **Write `docs/TestScript.md`** — the green-team/red-team checklist from Step 5, made concrete against
  this extension's actual endpoints, plus one green-team case per parent→child entry point and one
  per number-series field from **Standards §11.3**, for a human tester to run end to end at Step 7. With more
  than one required language, add a **language pass**: key pages, messages, and customer-facing
  documents walked once per required language, checking for untranslated text, truncation,
  regional terms, and formats. Each case names the glossary terms the tester should see, so a
  tester who reads English can run it without a translated copy.
- **If the project has AL test codeunits, run them — don't leave it to the human.** The AL MCP
  Server's `al_run_tests` runs them against the sandbox, one codeunit per call (**Ops § Automated
  Tests**), never against production. A failing test fails this step like a compiler error. If the
  server isn't connected, say so and list the codeunit IDs rather than reporting untested code as
  tested.
- **Translated documents aren't produced here.** Step 7 produces them once its functional test
  pass is green, so a fix found in testing doesn't make every translated copy stale too.

**Outputs:** `docs/DesignDoc.md` (updated in place, glossary included), `docs/Docs.md`, and `docs/TestScript.md`.
Together with `docs/ChangeLog.md` from Step 1 onward, that's Lite's four maintained documents; translated
documents follow at Step 7. Step 1's `docs/ProblemStatement.md` and
`docs/ProjectParameters.md` are also tracked, but written once at kickoff rather than kept current.

**Exit gate:** Every gap classified and resolved or explicitly deferred (logged in
`docs/ChangeLog.md`); dead-code scan clean; no obsolete references; `docs/Docs.md`'s diagram renders;
`docs/TestScript.md` is executable by a non-developer; translation checks clean; any AL test codeunits
have been run and pass, or it's recorded why they couldn't be.
