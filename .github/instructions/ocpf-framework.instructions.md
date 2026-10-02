# BC App Build Routine — Lite Agent Runbook

## OnlyCopilotFans Agentic Dev Framework — Lite Edition

**Version:** 3.1.0.0 (Lite, derived from the full framework v4.1.0.0)
**Last Updated:** September 21, 2026

> Version history for this edition lives in `LITE_RunbookChangeLog.md`, tracked independently of
> the full framework's own `fullVersion/RunbookChangelog.md` (though a change to one often has to
> be reflected in the other) and of any one project built with it. Diagrams for this routine live
> in `LITE_RunbookSchematics.md`. If either file is not found, create it.

> This is the lightweight sibling of the full **OCPF BC Agentic Development Framework**
> (`fullVersion/BC_App_Build_Routine_Agent.md`, in the same repo). Same author, same underlying
> discipline — half the steps, one model doing all the work, and no ceremony that a
> 10-files-or-fewer project doesn't need.

> **Companion documents, both fetched at Step 1 and shared unchanged with the full framework:**
> - `standardsGuide/ocpfALDevStandardsGuide.md` — the **OCPF AL Development Standards Guide**
>   (v1.10.0.0), cited as **Standards §**. Lite is *not* a reduced set of AL rules: the same rules
>   apply to a 5-file extension as to a 50-file one. What Lite reduces is *process*.
> - `opsGuide/ocpfOperationsGuide.md` — the **OCPF Operations Guide** (v2.0.1.0), cited as
>   **Ops §**: the procedures this routine uses — asking, intake, project setup, AL tools,
>   analyzers, symbols, editor sync, notifications, packaging, repository hygiene, translations,
>   fetched companions, documents, usage and cost, and the plugin.
> - `runbookSteps/` — **this runbook's step files**, one per step (`STEP-1.md` … `STEP-7.md`),
>   fetched from `liteVersion/steps/` at Step 1 alongside the guides. **The step sections below are
>   stubs; the step file is the step.** Read it in full the moment the step starts, never from the
>   stub or from memory.
> - `documentTemplates/` — one template per document (Design Doc, Human Effort Estimate, ChangeLog,
>   Docs, Test Script, Usage Report — the full set is Ops § Fetched Companions), fetched with the
>   step files; every document is written from its template
>   (Ops § Documents).
>
> **Read an Ops § section at the step that names it**, in that step's Actions or its exit gate —
> not the whole guide at once, and never from memory of a section you haven't opened here. This
> runbook states each rule in short form where you need it and cites the guides for the full
> version.
>
> **When to use Lite:** a Business Central AL Per-Tenant Extension with **10 or fewer AL files** —
> typically a handful of API pages over standard tables, maybe one or two new tables, a single
> developer or functional consultant driving it, one AI model doing the work. **Graduate to the full
> framework** the moment any of these stops being true: the object count grows past ~10, the project
> needs multiple sign-off roles (Dev Manager, Technical Lead, Functional Consultant as separate
> people), you want to split work across more than one AI model, or the extension is heading to
> AppSource, which tends to demand the fuller documentation trail. Nothing is lost by switching
> later — Lite's Design Doc and ChangeLog map directly onto the full framework's TDD and ChangeLog.
>
> **How the agent uses it:** work the phases in order (DEFINE → DESIGN → BUILD → PROVE). Don't
> start a step until its predecessor's exit gate is met. The *Project Parameters* block in Step 1
> is the single source of truth for every name, ID, version, and quoting decision — never hardcode
> any of those values in AL; always derive them from that block. It is persisted as
> `docs/ProjectParameters.md`, not just discussed — every later step reads it from
> that file. At the start of every session, read `.ocpf/notifications.json` and keep notifying the
> human the way it says; if it's missing, ask how they want to be notified (ALL ALONG →
> Notifications).
>
> **One model does everything.** Lite drops the full framework's Main/Light/Reasoning role split.
> There's no delegation to configure — the executing agent plans, generates, reviews, and documents,
> in that order, within each step. Step 1 still asks which model and thinking effort that one
> agent runs on, and confirms the session matches (Ops § Roles).
>
> **Every project document lives in `docs/`.** `docs/ProblemStatement.md`, `docs/ProjectParameters.md`,
> `docs/DesignDoc.md`, `docs/HumanEffortEstimate.md`, `docs/ChangeLog.md`, `docs/Docs.md`,
> `docs/TestScript.md`, `docs/UsageReport.md`, and every translated
> copy — created at Step 1, before the first document is written. Of the routine's own outputs, only
> `requirements/`, `app.json`, the AL source, `Translations/`, and `outputAppPackage/` stay in the
> project root. Every step names
> its documents with the `docs/` prefix; a bare name still means the `docs/` path. At every exit
> gate, list the root: a document sitting there is moved with `git mv` and the move noted in
> `docs/ChangeLog.md`. On a real project the design documents landed in the root because the steps
> named files without their folder.
>
> **Prime directive:** an ambiguous input produces ambiguous code. If a step's inputs are
> incomplete or contradictory, stop and ask the human — do not invent rules to fill the gap.
>
> **Installed by the OCPF plugin?** If this project has a `.ocpf/framework.json` file, it was set
> up by the OnlyCopilotFans agent plugin. Read ALL ALONG → OCPF Plugin before anything else in this
> session: it adds a once-per-session framework update check, a Standards Guide fallback,
> one-step AL tool setup, and the `documents` and `usage` skills. Without that file, ignore that section. Everything else here works
> the same either way.

### Setup

Same as the full framework: drop this file into the project root and rename it so your tool picks
it up automatically — `CLAUDE.md` for the Claude Code plugin in VS Code, or
`.github/copilot-instructions.md` for GitHub Copilot Chat. Keep this file itself as your master
copy elsewhere if you maintain more than one project.

You don't need to copy the Standards Guide by hand — Step 1 fetches it from the framework
repository into `standardsGuide/` and gitignores it there.

**Or use the OCPF plugin** (optional). The framework is also an agent plugin, `ocpf-bc`, for
Claude Code, GitHub Copilot, and other tools; the repository's `README.md` has install steps. Its
`start` skill does this setup for you: it helps choose Lite or Full, installs the latest runbook,
and checks the AL MCP Server tooling. See ALL ALONG → OCPF Plugin.

**Getting started prompt:**
> This AL project (10 files or fewer) will follow the Lite Agentic Development Framework outlined
> in `LITE_BC_App_Build_Routine_Agent.md` (may also be referred to as `CLAUDE.md` or `.github/copilot-instructions.md`). Please review this file and let's get started.

---

## Operating Rules (apply in every phase)

1. **The Project Parameters block (Step 1) is authoritative.** Publisher, prefix, namespace,
   versions, ID range, localization — read them from there and derive everything else. Never
   hardcode.
2. **Verify against BC symbol files, not memory.** Table numbers, `using` namespaces, field IDs,
   `ObsoleteState` — confirm each in the symbol file named in the Parameters block. Agent
   knowledge of BC table numbers is not reliable. The agent downloads those symbols itself at
   the end of Step 1 (ALL ALONG → Symbols); the human never has to. **Fallback** when the
   downloaded symbols don't answer: Microsoft Learn's Base Application and System Application
   reference (**Standards Appendix B**; links in ALL ALONG → Reference Sources). The downloaded
   symbols win when the two disagree.
3. **Treat the whole extension as one batch — two only if there's a natural split** (e.g., "setup
   + master data" vs. "documents"). At 10 files or fewer there is rarely a reason for the full
   framework's multi-batch phasing. Order objects within the batch so lookup/reference tables
   precede the entities that reference them.
4. **Lint everything as it's written; don't compile until generation is finished.** Run Step 3's
   pre-flight checklist on every object as it's generated, both passes, including symbol
   verification (Rule 2). The whole extension compiles and packages once, at Step 5, the moment
   Step 4 finishes. After that, every code change goes through the same cycle: compile, package,
   deploy to a sandbox, test, fix, repeat. Treat any compile error as systemic: fix the rule, then
   every file it touched.
5. **Zero errors, zero warnings before PROVE.** A warning is a defect. Step 5's compile must reach
   0/0, with the analyzers and nothing suppressed (ALL ALONG → Analyzers), before Step 6 begins.
6. **Human-in-the-loop is a feature — approve in batches, not one click at a time.** Pause for
   human approval before:
   - **generating code** — once, for the batch plan at Step 3, which covers both batches when there
     are two, unless the human chose to be asked before the second;
   - **applying root-cause fixes** — all the diagnoses from one test round or review, presented
     together for one decision (Step 5), with any fix that changes a design rule in `docs/DesignDoc.md`
     asked separately;
   - **finalizing the Design Doc;**
   - **installing any tool or runtime.**

   An approved run-through still stops by itself on any pre-flight failure or deviation from
   `docs/DesignDoc.md`, and the human can say "stop" at any time.
   6a. **Ask decisions in a selectable options box, not in prose.** When the agent needs the human
       to *decide* something — pick a design option, approve a version bump, resolve an ambiguity —
       present it through the interactive multiple-choice mechanism the agent's harness provides
       (e.g., Claude Code's `AskUserQuestion`), recommended option first with a short reason. Don't
       wrap ordinary progress (finishing a step, reporting a clean compile) in this — that's just
       noise. **Every intake question counts as a decision**, including values only the human
       knows: names, publisher, prefix, namespace, localization, ID ranges, versions. Ask them
       through the options mechanism, never as open-ended questions or a numbered list in chat;
       the mechanism's free-text entry (Claude Code: *Other*) carries a typed answer. Step 1 says
       what to offer. **Which mechanism each harness offers, and the option-count limits, are
       Ops § Asking and Approvals.**
   6b. **Don't install tooling without asking — and look harder first.** Before concluding a
       required compiler/runtime is missing, check whether the human's own IDE already provisions
       one privately (e.g., VS Code's AL extension gets its .NET runtime from a companion
       extension, not a system install). Installing anything is itself a human-in-the-loop
       decision regardless of what a fallback elsewhere in this runbook lists as available.
   6c. **From Step 5 onward, check in only after a step that hands the human something to act
       on.** Step 5 always does (a tested package): once its exit gate is met, put the choice
       through the selectable-options mechanism (Rule 6a) — proceed directly into Step 6, or stop
       here so the human has room to review, run, or publish the package. State how to resume. A
       step that produces nothing the human must act on yet gets one line instead (e.g., "done;
       starting Step 6 — say stop to pause"). Steps 1–4 are unaffected — Rule 6's own approval
       gates pace them.
       **The Step 6 → Step 7 boundary is a special case of this rule, not an addition to it** —
       see Step 7's own hand-off note, which replaces this generic check-in for that one
       transition. Don't do both.
   6d. **Zero-install first — never turn setup into the human's job.** Before proposing any
       install, or any manual setup (editing `PATH`, a shell profile, or an environment variable),
       work through the ladder in **Ops § Asking and Approvals**: what the editor already provides,
       then what's already installed, then — only then — an install asked for under Rule 6b.

       **Never hand the human a setup task the agent can do.** Connecting the AL tools, downloading
       symbols, keeping the editor's view current, and setting up notifications are the agent's job;
       the human approves the AI tool's prompts and signs in when a tool reaches a live Business
       Central environment. **Never send the human to the Command Palette to set up the AL MCP
       Server** — the AL Language extension has no such command — and never route AL tooling through
       a third-party VS Code extension.
7. **Log every deviation immediately.** Any departure from the Design Doc — human or agent — goes
   in `docs/ChangeLog.md` before the next batch starts.
8. **Work in the human's chosen working language.** The first question of Step 1 asks which
   language the human wants to work in. Every later question, options box, and explanation is in
   that language. **Always in English, regardless:** this runbook, the Standards Guide, AL code
   and names, commit messages, `docs/DesignDoc.md`, and `docs/ChangeLog.md`. **Kept verbatim in their
   original language:** raw requirements and tester feedback. When the human names a BC concept in
   their own language, map it through the glossary in `docs/DesignDoc.md` rather than guessing.

---

# PHASE: DEFINE

Goal: turn a business need into a validated scope and a filled-in parameter sheet — before any
design work.

## STEP 1 — Define the Problem & Lock Parameters

**Read `runbookSteps/STEP-1.md` in full before starting this step.** It carries the opening questions in
order and the complete **Project Parameters** table.

**In one line:** ask the working language, fetch both companion guides and the step files, ask how
to be notified, ask the Main model and effort (`roles` skill in a plugin project), ask the Copilot
session settings if Copilot is in use, create `docs/`, capture raw requirements verbatim, write the
problem statement and entity list with a quick gap check, then fill every parameter through the
options mechanism (Ops § Intake) and set up the AL project yourself (Ops § Project Setup).

**Outputs:** `standardsGuide/`, `opsGuide/`, `runbookSteps/`, and `documentTemplates/` (all fetched, gitignored), `.ocpf/usage.json` (Step 1's start timestamp) and `docs/UsageReport.md` (from its template), `.ocpf/copilot.json` and the Copilot keys in `.vscode/settings.json` (when Copilot is in use), `requirements/` (if any raw input was
captured), `docs/ProblemStatement.md` (purpose, scope, out-of-scope, entity list, open questions),
`docs/ProjectParameters.md` (the completed Project Parameters block, all placeholders
replaced, the Main model and effort included), `docs/` itself (created before the first document),
`.gitignore` populated per the table above and verified with `git check-ignore`, `app.json`, and
`.alpackages/`.

**Exit gate:** Every question was asked through the options mechanism. `app.json` matches the
sheet, and the target version's symbols are in `.alpackages/` (Ops § Project Setup). Both companion guides are present, in `standardsGuide/` and
`opsGuide/`, and gitignored. The notification choice is recorded in
`.ocpf/notifications.json`, applied, and tested. `docs/ProjectParameters.md` exists, in `docs/`, with no placeholder remaining, and the Main model and effort recorded there match what this session is running on. Every `.gitignore` entry from the framework-files row is present and `git check-ignore` confirms each existing framework file — `LITE_RunbookChangeLog.md` included — is ignored. Nothing from the `docs/` list is in the project root. Deployment Target
is one allowed value. Namespace is consistent or correctly N/A. If
Permission Sets required = `Yes`, ≥ 2 IDs are reserved. Onboarding questions are each answered.
Every target language is classified against Microsoft's live page and, unless source wording is
*US wording, no translation files*, has a required-at-release answer and a named reviewer; source
language and wording are recorded. Human confirms the sheet.

---

# PHASE: DESIGN

Goal: one self-sufficient Design Doc, sanity-checked, before any code.

## STEP 2 — Write the Design Doc & Self-Check

**Read `runbookSteps/STEP-2.md` in full before starting this step.** It carries the Design Doc's two
halves, the API caption-locking questions, and the self-check list.

**In one line:** write **one** self-sufficient document, `docs/DesignDoc.md` — Part A what and why,
Part B how, glossary included — using the `documents` template; produce
**`docs/HumanEffortEstimate.md`** in the same pass (ALL ALONG → Usage & Cost Tracking); then run the
self-check.

**Outputs:** `docs/DesignDoc.md`; an **Object Register** table (inside `docs/DesignDoc.md` is fine at this
scale — every planned object with its ID, source table, and R/W status); `docs/HumanEffortEstimate.md`.

**Exit gate:** Human sign-off. Self-check passes with 0 blocking issues. No rule requires
knowledge outside the document. `docs/HumanEffortEstimate.md` exists, covers every object, and states
its assumptions.

---

# PHASE: BUILD

Goal: generate AL, lint clean — including symbol verification — then compile, package, test, and
fix in a loop until clean.

## STEP 3 — Plan & Scaffold

**Read `runbookSteps/STEP-3.md` in full before starting this step.** It carries the pre-flight checklist
Steps 4 and 6 reuse.

**In one line:** scaffold the project structurally (analyzer settings, `.gitignore` block, `scripts/`
— not compiled, Operating Rule 4), fetch the BCQuality snapshot and the patterns library, write the
batch plan, and get one approval for it.

**Outputs:** Batch plan, project scaffold, the pre-flight checklist.

**Exit gate:** Batch plan approved (and, with two batches, the run-through choice recorded);
scaffold structurally complete, analyzer settings and (for AppSource) `AppSourceCop.json` in place per Ops § Analyzers
(not compiled — Operating Rule 4); pre-flight checklist ready.

## STEP 4 — Generate the Code

**Read `runbookSteps/STEP-4.md` in full before starting this step.**

**In one line:** generate batch by batch exactly per `docs/DesignDoc.md`, lint each batch as written,
log every deviation in `docs/ChangeLog.md` before the next batch, commit each batch separately.

**Outputs:** Every AL file, lint-clean including symbol verification; ChangeLog entries for any
deviation from `docs/DesignDoc.md`. The extension is **not** compiled yet.

**Exit gate:** Every planned object generated and pre-flight-clean; permission-set coverage
verified. A clean compile is not required to close this gate — Step 4 hands off directly into
Step 5's mandatory compile-and-package.

## STEP 5 — Compile, Package, Test & Iterate

**Read `runbookSteps/STEP-5.md` in full before starting this step.**

**In one line:** the first full compile-and-package with the analyzers, then the cycle — compile,
package, deploy to a sandbox, test, diagnose (check `patterns/` first), fix, repeat — to 0 errors /
0 warnings; translations drafted and reviewed inside this cycle (Ops § Translations).

**Outputs:** All files compiling and packaging with **0 errors, 0 warnings**; at least one package
published and manually tested on a sandbox; `docs/ChangeLog.md` current; `docs/DesignDoc.md` updated for
every rule change.

**Exit gate:** Full extension compiles clean, with the analyzers and nothing suppressed (Ops § Analyzers); human confirms sandbox testing is clean; no known
systemic issue outstanding; no unit in a language required at first release is
`needs-translation` or `needs-adaptation`, and translation checks are clean.

---

# PHASE: PROVE

Goal: confirm the built code matches the Design Doc, is clean, is documented, and has passed a
human-run release test.

## STEP 6 — Review, Gap-Check & Finalize Docs

**Read `runbookSteps/STEP-6.md` in full before starting this step.**

**In one line:** gap-check the built code against `docs/DesignDoc.md`, review it against the
Standards Guide, AL Guidelines, and BCQuality, apply the fixes through the compile cycle, bring the
Design Doc to as-built, and write `docs/Docs.md` and `docs/TestScript.md` (`documents` templates).

**Outputs:** `docs/DesignDoc.md` (updated in place, glossary included), `docs/Docs.md`, and `docs/TestScript.md`.
Together with `docs/ChangeLog.md` from Step 1 onward, that's Lite's four maintained documents; translated
documents follow at Step 7. Step 1's `docs/ProblemStatement.md` and
`docs/ProjectParameters.md` are also tracked, but written once at kickoff rather than kept current.

**Exit gate:** Every gap classified and resolved or explicitly deferred (logged in
`docs/ChangeLog.md`); dead-code scan clean; no obsolete references; `docs/Docs.md`'s diagram renders;
`docs/TestScript.md` is executable by a non-developer; translation checks clean; any AL test codeunits
have been run and pass, or it's recorded why they couldn't be.

## STEP 7 — Release for Testing

**Read `runbookSteps/STEP-7.md` in full before starting this step.** It carries the hand-off moment and
the release gate.

**In one line:** the human runs `docs/TestScript.md` in every required language, plus the upgrade
path unless this is the first release; every finding goes to `docs/ChangeLog.md` verbatim and is
triaged; the package that passes ships; the production deploy is the human's.

**Outputs:** `docs/ChangeLog.md` updated with every test finding and its resolution.

**Exit gate:** All green-team tests pass; all red-team tests fail gracefully; the upgrade path
passes, or this is the first release (**Standards §9.6**); permission sets
verified; every translated document Step 1 asked for exists and has been reviewed; every required
language has passed its language pass and its state scan shows every unit `signed-off` or `final`
(Ops § Translations).
**If everything passes, the package that was actually tested is the one deployed to Production** —
bump its Build segment (e.g. `0.0.5.0` → `0.0.5.1`) or copy it to an immutable
filename first, so the shipped artifact stays permanently identifiable. This is marking the
release candidate, not building a new one — no recompile, no new testing required to do it.
**Before that deploy, restate the Schema Sync Mode assessment** (ALL ALONG → Packaging &
Versioning) — **Add** if this release is additive-only, **Force Sync** with an explicit data-loss
warning if anything was removed, shrunk, retyped, or re-keyed since the last production release.

---

# ALL ALONG — Continuous Discipline

Run these throughout, not as a final step.

**Each section below carries its non-negotiables and points at the Operations Guide for the
procedure.** Read the named **Ops §** section at the step that needs it.

## ChangeLog.md — the single running log

Lite merges what the full framework splits across a ChangeLog, a Testing Feedback Log, and a
Roadmap into **one file**. Every deviation from `docs/DesignDoc.md`, every diagnosed root cause, and
every piece of testing feedback goes here, before the next batch or the next test round begins.
Entry format:

```
## <Type: Issue / Feedback / Deferred> <SeqNo> — <Short Description>
**Problem:** What was wrong, missing, or requested.
**Root cause:** Why it happened (Issue/Feedback only).
**Resolution:** What was changed, or why it was deferred/rejected.
**Files affected:** List of changed files.
**Design Doc updated:** yes/no.
```

**Name the person, not a role.** When a decision or preference is attributed to a human, write
their actual name — never a generic "the human" or "the user." The moment a second contributor
joins the project, a role-noun stops answering the only question that phrase exists to answer.

Commit each batch to version control separately, before the next begins, with a message that
references its `docs/ChangeLog.md` entries.

## Usage & Cost Tracking — `docs/UsageReport.md`

**Full procedure: Ops § Usage & Cost.** Read it at Step 1 (when the timestamps start) and at every
exit gate. Lite keeps no `ProjectProgress.md`, so the usage table lives in `docs/UsageReport.md`. In
a plugin project the `usage` skill (`/ocpf-bc:usage`) does the measuring.

- **Every step start and every exit gate is timestamped** in `.ocpf/usage.json`; nothing can be
  attributed to a step without them.
- **Measured, never estimated.** Claude Code's per-request usage — input, output, cache-write, and
  cache-read tokens per model — is read from its session transcripts on disk. GitHub Copilot exposes
  no token counts: record **premium requests** per step from the human's Copilot usage page, and say
  so. Never invent a token figure.
- **Cost uses published prices, fetched, dated, and cited** — stored in `.ocpf/pricing.json` with the
  source URL and the date read; never hardcoded.
- **Refresh the table at every exit gate:** one row per step — model(s), input, output, cache
  write, cache read, cost, elapsed time, turns, human decisions asked, sub-agent calls (the skills'
  delegations) — next to the hours from
  `docs/HumanEffortEstimate.md` (Step 2) once it exists.
- **Say what couldn't be measured** — `n/a` with the reason, never a blank or a guess.

## Packaging & Versioning

**Full procedure: Ops § Packaging.** Read it at Step 5's first package and before Step 7's release
bump. The non-negotiables:

- **Fixed name and location:** `<ExtensionName, spaces → underscores>_<version>.app` in
  **`outputAppPackage/`**, read from `app.json` at build time. Say the exact path every time a build
  completes.
- **Built packages are tracked, never gitignored** — no blanket `*.app` entry.
- **The agent never publishes to a production environment.** Before every publish, state the target
  environment and type; if it isn't a sandbox, stop and ask. Never force a destructive schema change
  (`ForceSync`, `Recreate`, `forceUpgrade`) without a separate approval naming what can be lost. The
  production deploy at Step 7 is the human's, through Extension Management (Ops § Packaging).
- **Never delete or overwrite a package from a different version.** Repeated builds at the same
  in-progress version legitimately overwrite that file; the protection is across versions.
- **The package that passes Step 7 ships** — bump its Build segment or copy it to an immutable name,
  and say which one passed.
- **Version bumps are proposed and approved**, never a silent `app.json` edit.
- **Flag Schema Sync Mode on every completed build:** **Add** for additive-only, **Force Sync** with
  a data-loss warning otherwise.

## OCPF AL Development Standards Guide

Two companions are fetched at Step 1 and kept for the life of the project: the **Standards Guide**
(`standardsGuide/ocpfALDevStandardsGuide.md`, v1.10.0.0), cited as **Standards §**, and the
**Operations Guide** (`opsGuide/ocpfOperationsGuide.md`, v2.0.1.0), cited as **Ops §** and shared
unchanged with the full framework.

**Full procedure: Ops § Fetched Companions** — fetching, refreshing, the plugin's offline copies,
and what to do when GitHub is unreachable.

- Both live inside the project root and are **always gitignored** — not covered by Step 1's
  framework-files question. **Fetched with them, and ignored the same way:** this runbook's step
  files into `runbookSteps/` (from `liteVersion/steps/`) and the document templates into
  `documentTemplates/`. A step file whose `**Runbook version:**` line doesn't match this runbook is
  skew: say so and refetch.
- **Neither is optional:** a missing copy is a real gap. Say so and ask the human for a copy rather
  than working from memory of a rule.
- **Version skew is named, not papered over.**

## Repository Hygiene

**Full procedure: Ops § Repository Hygiene.** Read it at Step 1 and Step 3.

**Always gitignored, not a per-project choice:** `.claude/settings.local.json`,
`.ocpf/notifications.json`, `.ocpf/copilot.json`, `.ocpf/usage.json`, `.ocpf/pricing.json`,
`standardsGuide/`, `opsGuide/`, `runbookSteps/`, `documentTemplates/`, `patterns/`, `scripts/`,
`.alpackages/`, `*.g.xlf`, and Microsoft's translation files. BCQuality lives outside the project root entirely.

**Always tracked:** `docs/DesignDoc.md`, `docs/ChangeLog.md`, `docs/Docs.md`, `docs/TestScript.md`,
`docs/HumanEffortEstimate.md`, `docs/UsageReport.md`, the Step 1 kickoff artifacts, the AL source, `Translations/*.xlf`, and every package in `outputAppPackage/`.

**Gitignored by default, with Step 1's question — by exact filename, every one:** this runbook
under whatever name it was placed (`CLAUDE.md`, `.github/copilot-instructions.md`,
`.github/instructions/ocpf-framework.instructions.md`, `LITE_BC_App_Build_Routine_Agent.md`), its
changelog in either spelling (`LITE_[Rr]unbook[Cc]hange[Ll]og.md`), `LITE_RunbookSchematics.md`, and
the plugin's `.ocpf/` folder — except `.ocpf/notifications.json`, always ignored. Step 1's parameter table (in
`runbookSteps/STEP-1.md`) has the list; Ops § Repository Hygiene has the block to paste. **Verify with `git check-ignore -v`** on
each file that exists — that command, not a re-read of `.gitignore`, is what proves an entry works.
**Never the project's own `docs/ChangeLog.md`**, one of Lite's four maintained documents.

**`docs/` is where every document lives** — see the header note. At every exit gate, list the root:
a document sitting there is moved with `git mv`, and the move is noted in `docs/ChangeLog.md`.

**If a project built on an earlier Lite version has `docs/ChangeLog.md` in `.gitignore`,** remove that
entry and commit the file — earlier wording said "this runbook and `docs/ChangeLog.md`" when it meant the
framework's changelog. Check for a remote first: collaborators will see the file as newly added.

**If any of this is already tracked,** add the entry, then untrack with `git rm --cached` — checking
for a remote first, since collaborators will see the files as deleted.

## AL MCP Server

**Full procedure: Ops § AL Tools.** Read it at the end of Step 1, when the tools are first needed.

- Nothing is installed: Copilot Chat has the AL tools built in; Claude Code and Copilot CLI use the
  extension's bundled AL MCP Server. With the plugin, `al-mcp-setup` does it in one step.
- **The human only approves the AI tool's prompts.** No Command Palette command exists for this, and
  no third-party bridge extension is used.
- A server registered mid-session appears only next session; until then use the one-shot helper.

## Analyzers

**Full procedure: Ops § Analyzers.** Read it at Step 3's scaffold and at Step 5's compile.

- The mandatory compile runs **CodeCop**, **UICop**, and exactly one of **PerTenantExtensionCop**
  (SaaS/OnPrem PTE) or **AppSourceCop** (AppSource) — never both.
- `.vscode/settings.json`, and `AppSourceCop.json` for AppSource, are created at Step 3.
- Outside Copilot Chat, run `scripts/al-analyze.*`. The AL MCP Server's `al_build` never applies
  analyzers (observed on AL extension 18.0.2732683; re-verify on a newer release), and its
  `al_compile` only does so with one exact argument shape — anything else
  returns a clean pass on failing code, and it produces no `.app` (**Ops § Analyzers**).
- **Read the warnings, not just the result** — `al-analyze` exits `3` on warnings, and any warning
  fails Rule 5. No suppressions, and no ruleset.

## Symbols

**Full procedure: Ops § Symbols.** Read it at the end of Step 1.

- **The agent downloads symbols; the human never does.** Global sources need no sign-in; a sandbox
  download (one browser sign-in) covers non-AppSource dependencies and localized projects.
- The AL MCP Server's global download is **W1 only**.
- **Confirm symbols by using them**, never by unpacking `SymbolReference.json`. A sandbox download
  carries Microsoft's own source and translation files: read them, never commit them.

## Keeping the Editor in Sync

**Full procedure: Ops § Editor Sync.** Read it at the end of Step 1 and after every clean compile.

- Red marks the latest compile didn't report mean the editor's view is stale, usually after
  `app.json` changed on disk or symbols were downloaded outside VS Code.
- **Detect** with the diagnostics tool; **fix** by re-downloading symbols in Copilot Chat, or by
  asking the human, at the end of the reply, to run **Developer: Reload Window**.
- **Never change code that compiles clean to clear stale marks.**

## Notifications

**Full procedure: Ops § Notifications.** Read it at Step 1, right after the working language.

- **Ask once, as one multi-select question:** *Claude app*, *Sound*, *Desktop notification*, any
  combination, or *No notifications* — offering only what works for this tool, OS, and sign-in.
- **Record it in `.ocpf/notifications.json`** (per developer, always gitignored) and read it at the
  start of every session; ask again when it's missing.
- **Apply it** through each tool's own notifications and hooks — never a script-raised banner on
  macOS — and test it once.

## Reference Sources — Microsoft Learn and AL Guidelines

**The list: Ops § Reference Sources** — Microsoft Learn's Base Application and System Application
references, the translation-files and country/language pages, Microsoft's terminology collection and
style guides, and AL Guidelines. Consulted online; nothing is fetched.

- **Precedence:** downloaded symbols beat Microsoft Learn on anything symbol-verifiable; the
  Standards Guide beats AL Guidelines on any AL rule. Surface a conflict rather than picking a side
  silently.
- **No web access? Say so** rather than answering from memory.

## BCQuality Knowledge Snapshot

**Full procedure: Ops § Fetched Companions.** Read it at Step 3, when the snapshot is fetched, and
at Step 6, when it's used.

- A third-party BC AL code-quality knowledge base, fetched once at Step 3 and refreshed only on
  request.
- **It lives outside the project root** — `alc` would otherwise compile its illustrative snippets —
  so there's nothing to gitignore.
- At Step 6 it's an independent review pass whose findings are integrated like any other, never
  applied blind.

## OCPF BC AL Patterns Library

**Full procedure: Ops § Fetched Companions.** Read it at Step 3, when the library is fetched.

- The human's own cross-project BC AL patterns, fetched once into `patterns/`, always gitignored,
  refreshed only on request, and merged rather than overwritten.
- **Using it — before writing, and before diagnosing.** At Step 4, before generating any child list
  or list part, setup page, or wizard, read Standards Part 11 and the matching pattern files (Step 3
  marks those objects). At Step 5 or 7, check whether `patterns/` already documents a problem
  before diagnosing from scratch.
- A fix likely to recur on future projects is a candidate for a new pattern — flag it, don't add it
  unilaterally.

## Translations & Terminology

**Full procedure: Ops § Translations** — the glossary, the cycle inside Step 5, review and approval,
and the release gate. Read it at Step 1, at Step 5's first full build, and at Step 7's gate. Skip
everything here if Step 1 chose *US wording, no translation files*.

- **The glossary lives in `docs/DesignDoc.md`** and is filled only by **Standards Appendix D**, never from
  model memory.
- **The agent drafts, a named person approves.** Every approval is logged in `docs/ChangeLog.md` by name
  (**Standards §8.7**).
- **The release gate is a plain state scan:** every unit in every language required at first release
  is `signed-off` or `final`.
- **Changed source text invalidates approval** — the unit goes back through drafting and review.

## Permission Sets

- **Required the moment this project owns one table** (**Standards §5.3**).
- **Coverage is verified at the pre-flight checks in Steps 3 and 4**, so a gap doesn't wait for the
  compile. Step 6 relies on the last 0/0 compile, after confirming it ran with the analyzers and
  nothing was suppressed (ALL ALONG → Analyzers).
- **Named for this extension, not just the prefix:** `<PREFIX> <APPCODE>, VIEW` and
  `<PREFIX> <APPCODE>, EDIT`, 20 characters or fewer (**Standards §5.4**).

## OCPF Plugin (Optional)

**Full procedure: Ops § Plugin.** Read it at the start of a session in a plugin-installed project.

**Applies only when `.ocpf/framework.json` exists** — the plugin's `start` skill creates it. Without
that file, skip this section.

- **Once per session,** compare the project's runbook version against the latest published Lite
  runbook and offer **Update now / Not now / Skip this version**. Never replace the runbook without
  an explicit yes.
- **What the plugin adds:** offline copies of the runbooks, both companions, the step files, and the
  templates; one-step AL tool setup; notification setup; and the `status`, `al-standards`,
  `notifications`, `roles` (asks Lite's two model questions), `documents` (writes any document from
  its template), and `usage` (measures tokens and cost per step) skills. (The `ocpf-light` and `ocpf-reasoning` sub-agents
  belong to the full framework's role split, which Lite doesn't use.)

## Step Map — Lite vs. Full Framework

| Lite step | Full framework equivalent |
|---|---|
| Step 1 — Define & Lock Parameters | PRE-01, PRE-02, Step 01 |
| Step 2 — Design Doc & Self-Check | Step 02 (FRD), Step 03 (TDD), Step 04 (Sanity Check) |
| Step 3 — Plan & Scaffold | Step 05 |
| Step 4 — Generate the Code | Step 06 |
| Step 5 — Compile, Package, Test & Iterate | Step 07 |
| Step 6 — Review, Gap-Check & Finalize Docs | Step 08 (Gap-Fit), Step 09 (Code Review), Step 10 (Update Docs), Step 11 (Document) |
| Step 7 — Release for Testing | Step 12 |

**Shared with the full framework, not reduced:** the OCPF AL Development Standards Guide. Both
editions fetch the same v1.10.0.0 file and apply the same AL rules — Lite differs only in process.

**Document count:** 4 maintained documents (`docs/DesignDoc.md`, `docs/ChangeLog.md`, `docs/Docs.md`,
`docs/TestScript.md`), plus Step 1's two kickoff artifacts (`docs/ProblemStatement.md`,
`docs/ProjectParameters.md`) and two records (`docs/HumanEffortEstimate.md`, `docs/UsageReport.md`),
versus the full framework's 23 (`ProblemStatement`, `ProjectParameters`, `FRD`, `TDD`,
`SanityCheck`, `HumanEffortEstimate`, `BuildPlan`, `ObjectRegister`, `ChangeLog`, `ProjectMemory`, `ProjectProgress`, `GapAnalysis`, `CodeReview`,
`PostDevTDD`, `Documentation`, `UserGuide`, `HumanUnitTestScript`, `Deployment`,
`AutomatedTestScripts`, `ReleaseTestResults`, `TestingFeedback`, `Roadmap`,
`TranslationGlossary`). Lite keeps its translation glossary inside `docs/DesignDoc.md`; translated copies
of `docs/Docs.md` and `docs/TestScript.md` don't count as separate documents.

---

*Lite Edition derived from the OCPF BC Agentic Development Framework, created by AJ Ansari,
Microsoft MVP, OnlyCopilotFans. Update this runbook when the full framework changes in a way that
should flow down to Lite.*
