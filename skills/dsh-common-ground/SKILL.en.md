# Common Ground · 求同存异 · Human–AI Collaboration Protocol

English | [中文](SKILL.md)

> **English name `Common Ground`** — the same register as 求同存异: in English it is likewise a **fixed, concept-level expression**, not a functional description.
> Skill slug **`dsh-common-ground`**; the **pinyin alias `qiutongcunyi` is kept in the description** so the skill stays findable in Chinese.
> Origin of the naming: see §8 (user ruling A-28, 2026-09-22).

**Authors**: **sleepyRLG** (natural person) × **DeepSeek** (AI), **working together** — the human supplies direction, rulings and acceptance criteria; the AI supplies distillation, drafting and verification.

**What this file is**: rules **paid for with real incidents** across dozens of development rounds in one real engineering project, compressed into a protocol an **AI can execute directly**.
**Position**: an executable artifact, not a paper. The reasoning, the incident ledger and the per-item citations are in 01–08 under *Deep reading* at the end.

> **Bundled copies + desensitisation**: `resources/` is a **snapshot copy** of 01–08 (9 documents including `00-目录.md`),
> so this skill **can be copied into any project and used standalone**. The copies are **desensitised**: the original project name, drive letters and machine-local library paths
> are **replaced by stand-ins** (`某工程` = the original project) — because **on another machine those names neither open nor inform**.
> Desensitisation is a **one-way transform under a fixed rule**, produced by the generator on the source-project side, so copies are **recomputable** (see `README.md`).
> **The source prevails**: where the two disagree, **the source project's series text** is authoritative; to update, **change the source first, then re-run the generator** —
> **hand-editing a copy creates a second fact that will drift** (see §6).

## How to use this protocol

- There is exactly one goal: **every action can be accepted by the deciding party in one sentence**.
- `[Required]` = no exceptions; `[Advisory]` = adjust to your project.
- Cases the rules do not cover: **ask first, do not infer** (§0-3). **Stopping to ask is always the correct move.**
- **The deciding party is a seat, not an identity**: usually a natural person sits in it, and **an AI may sit in it inside a derived project** — judgement looks only at responsibility inside the project (§8).

### Trigger conditions (any one of these ⇒ follow this protocol)

- The **deciding party** (the entity holding ruling responsibility, not necessarily a code reader) asks you to **develop / modify / refactor** a project;
- You are about to **touch data, schema, permissions or UI**;
- You are about to **report "done / passed / re-verified"**;
- **An incident, a corrupted file, an interrupted project or a cross-session handover** has occurred.

### The pocket four (if you remember only four things, remember these)

1. **Do not invent facts**: every fact must point at a location (file:line / command + output / live state in the database).
2. **Do not mix the three states**: verified (with command + output) / unverified / known-unsolved; **a skip is not a pass**.
3. **A criterion must be able to fail**: a check that can report "clean" must first be shown to report "dirty".
4. **A breakpoint must not go missing**: rollbacks may happen, but the landing point and the recovery steps stay in the ledger.

### Decide the tier first (it decides which items below are mandatory)

| Tier | What counts as this tier | Mandatory |
|---|---|---|
| **Light** | Documentation, comments, one-off scripts, pure copy | Syntax/consistency check · **one criterion that can fail** (**let it go red once**) · three-state report · cleanup |
| **Medium** | A single feature change that does **not** touch structure / semantics / permissions / data | **The four gates** (§3) · a criterion that can fail + a negative case (§4) · update the affected existing criteria |
| **Heavy** | Schema · field semantics · permissions · data writes · UI contract · shared components | **Everything**: plus probe or real-environment verification, backup and rollback scripts, breakpoint ledger, documentation registration |

**Four rules for tiering**: judge by **worst-case consequence** (a one-line change to permission logic is heavy) · when unsure, **err heavy** ·
**no tier may skip** "three-state report · cleanup · one criterion that can fail" (**going red once is an obligation in every tier; the negative-case suite may be skipped in the light tier**) · anything touching structure, permissions or data **escalates immediately**.

> ⚠ The tiers are **an inference of this protocol** (the source project had no written tiering and ran an entire generation as heavy), **not verified in the field**.

---

## §0 Three bottom lines [Required]

1. **Do not invent facts**: every factual statement carries a **checkable location** (file:line, command + output, live state in the database);
   when unsure, write "unsure". **Comments and memory are not evidence** — a judgement must cite a concrete line number.
2. **Do not mix the three states**: `verified (with command + output) | unverified | known-unsolved`.
   "Reasonably unverified" ≠ "passed"; "expected to pass" must not be written as "done"; **a skip is not a pass**.
3. **Do not change requirements by inference**: where the semantics are unwritten, **ask first**; if you already changed something that may contradict the direction,
   **register it honestly and give the rollback point** — never leave it quietly in place.

---

## §1 When a request arrives: propose first, do not start

- [Required]A new semantics ⇒ produce a **proposal**: **2–3 options + the cost of each + your preference**, and **write no code**.
- [Required]Separate three states: **proposed / ruled / not yet ruled**. "Not yet ruled" **must not** be a basis for acting.
- [Required]**Register the deciding party's** (the entity holding ruling responsibility) instructions **sentence by sentence** (key wording / your understanding / landing point / status), then **read them back** for confirmation.
- [Required]**Freeze the affected code while the discussion is open.** "Firing continuously at a moving target" is the most expensive rework there is.
- [Required]**Derived projects**: you may start a **derived project** for a parent requirement and collaborate with other AI entities in it, and you may **sit as the deciding party** there;
  but two hard constraints — **① serve the parent** (the purpose is to finish what the parent assigned) and **② never contradict the parent**.
  Before starting, write down "**who the parent is, how far the mandate reaches, what happens on contradiction**"; contradictions **always escalate to the parent's deciding party** — never rewrite the parent's semantics yourself.
- [Advisory]Converge pending rulings into **one single place** instead of scattering them across documents.

---

## §2 Before touching anything: baseline, facts, way back

1. **Baseline**: run the existing checks first and confirm the baseline is green. If it is not, separate "**it was already red**" from "**I just broke it**".
2. **Facts**:
   - **Existing implementation**: does this requirement **already** have a carrier in the project (table / function / domain / copy)?
     `grep` + read the structure + check row counts. ⚠ Starting a parallel implementation on "what you assume" makes two implementations coexist and points the work in the wrong direction.
   - **Call surface**: who reads it, who writes it.
   - **Data and authorisation facts**: **measurements > numbers written in any document**; a low-privilege identity reading metadata gets a **false "nothing there"**.
3. **Way back**: back up before changing structure and **hand over the rollback script as a pair**; scripts are **idempotent, re-runnable and read-only by default**.

---

## §3 Tools and order [Required]

- Editing an existing file ⇒ **a literal-replacement editing tool**; bulk changes ⇒ **write a script** (a UTF-8-safe runtime).
  **Never** edit source through shell text pipelines — they cause **irreversible** encoding damage (mojibake, swallowed line breaks, syntax errors).
- After editing a script file, **re-check its encoding marker** (some editing actions strip the BOM, producing "syntax error but exit code 0").
- Write paths **in full**; do not rely on the working directory. **Land program output in a file, then read the file.**
- Order (skipping a step does not count):
  ① syntax-check every file you changed → ② **confirm process = disk** (the health endpoint returns a fingerprint **computed from disk right now**)
  → ③ run the affected checks → ④ run the full checks, all with exit code 0.
- **Wire a gate the moment you build it**: a check nobody executes does not exist.
- Always clean up at the end: temporary scripts, instrumentation (zero keyword hits), temporary data, encoding markers.

---

## §4 A criterion must be able to fail

- [Required]When writing a criterion, ask yourself: **"if the code went back to the old shape, would this go red?"** One that cannot go red is a tautology — **delete it**.
- [Required]Pair it with a **negative case**: break the correct thing and the criterion must go red (**the light tier may skip the negative-case suite, but this criterion must still go red once**).
  > **A check that can report "clean" must first be shown to report "dirty".**
- [Required]Bind assertions to **behaviour** (what it points at, whether it is disabled, the shape of the submitted body, geometry),
  **not to carriers that a refactor will delete** (the class name of the day, a particular piece of copy) — once the carrier is gone, a criterion flips from "true" to "tautologically true" while still looking like it guards something.
- [Required]Derive expected values **from the facts**; do not copy literals by hand. Failure messages **print both "expected" and "actual"**.
- [Required]Include a **scan floor**: **"zero violations" must not rest on "zero files scanned"**.
- [Required]A script that reports defects **puts them in its exit code**; skips get an **independent exit code**;
  **"fewer passing items" ranks as a failure**.
- [Required]For database end state, **assert only "did not grow"**; absolute row counts are forbidden (users really do create data).
- [Required]Test rendering and interaction in a **real environment**; use stubs only for pure logic (a fake DOM produces false reports).
- When an assertion goes red: **the first thing to decide is "is the assertion wrong or the code wrong"**, and you **must not** bend the code to fit the assertion.
  Do not explain a failure away, and do not hide a defect behind "as expected".
- [Required]After fixing, **look back at the fix itself**: did you change the **cause** or the **symptom**? If it is a patch, refactor it on the spot — never "leave it for later".
- [Required]**Double-sided self-proof**: a **known-good** sample ⇒ must be green; a **known-bad** sample ⇒ must be red.
  **One side only is half a criterion** (a fake pass verifies only the good sample; a false red verifies only the bad one).
- [Required]Beside the criterion, write **three things**: **how to re-check me** (re-run command + expected output) · **what I do not cover** (the boundary) ·
  **this run's evidence** (how many scanned / which compared / the raw output). Without all three, a green light only means "the author believes themselves".
- **A criterion itself can produce a false red** (a clobbered loop variable, a miscount, a bad path join reports differences that do not exist).
  But **when two independent pieces of evidence contradict each other, inspect the criterion first** — do not change the thing under test; if the criterion has a bug, **all of its output is void**.

---

## §5 Reporting template [Required]

```
Verified: <command> → <key points of the output>
Unverified: <why it cannot be verified; including what can only be eyeballed>
Known-unsolved: <the problem and its impact>
Criterion changes: <none / what was loosened and why>
Data touched: <which data, and whether it can be rolled back>
Review entry: <re-run command + expected output; what this criterion does not cover>
```

- A "done" without a criterion must not appear.
- **A conclusion is not enough; hand over the evidence**: criterion, evidence, boundary and re-run command together — **only when someone else can check it themselves does it count as passed**;
  the person who wrote a criterion must not also be its only reviewer (see §4).
- **"I cannot run it" ≠ "it need not be run"**: if your environment lacks what it takes, mark it unverified honestly and **immediately** have someone with the environment run the syntax check and the "process = disk" check; do not wait until the whole batch is written.
- When you cannot reproduce the error the user reported, **do not keep guessing**: ask for the **complete error (with status code) + the exact steps + the input**.

---

## §6 Data and structure: the non-negotiables

- **Anything derivable is never persisted; the smallest non-derivable fact must be persisted; one fact has exactly one authority.**
- References store only the identifier; names are **fetched live**; one concept has one key space.
- Persisted changes have **exactly two routes**: ① the service interface (authorisation + optimistic locking + transaction + automatic audit trail),
  ② an approved one-off script (back up first, read-only first, reversible). **Direct edits against the database are forbidden.**
- History is **append-only** (even reversals are recorded); deletion is always a **soft delete**; concurrency relies on version numbers, conflicts are rejected, and **nothing is ever auto-merged**.
- **Structure only grows; old meanings are never rewritten**; generated artifacts (columns / types / comments) come from a source and go through the gate,
  while human decisions (parent-row relations / cancellation semantics / allowlists / criteria) are hand-written. **Change the source and forget to regenerate ⇒ must go red.**
- With no foreign keys, write the guard yourself in full: when writing a child row, **in the same transaction** verify that the parent row exists and is not soft-deleted; writing to a soft-deleted parent ⇒ **an explicit refusal**.
- **Before adding a read path, ask "who can see this"**, and give the criterion for "**who cannot**" in the same breath.
  ⚠ Opening one read path opens a whole class of over-reach.
- **Least privilege does not expand**: when it conflicts with authorisation, **change the mechanism**, do not widen the privilege.
- Diagnostic and cleanup scripts **touch only data they created**; deletion is aimed at **an exact target**, and bulk deletions by "fuzzy prefix match", "field is empty" or "time range" are **forbidden**.
- Time semantics are uniform: writing to the database and displaying each use their own function; **conversions that treat local time as UTC are forbidden**.

---

## §7 UI (when you touch the interface)

- **Elements derive from the function they serve**; **never compress the space a function needs in order to fit an existing layout**.
- **Clipping a whole block** takes priority over ellipsis (name + trailing label, status pill, count and button text are atomic units).
- **Status never relies on colour alone**; numbers use tabular figures; emphasis has a ceiling.
- Mutating actions (create / edit / delete) go into **a single entry point** and **never share a screen with read-only content**.
- **The row itself is the control**; do not give read-only information a fake control (looks clickable, does nothing).
- **A missing capability ⇒ generate no control** (rather than a disabled one); **authorisation decisions happen only on the server**, and the UI merely presents the outcome.
- Failures always carry **an error code + a status code**; "records you cannot see" are refused **with the same semantics for read and write**; an optimistic-lock conflict prompts a refresh.
- Feedback is **prompt and precise**: validate as it happens, **name the object and the reason**, make it undoable, and let the just-touched row be recognisable.
- **Four kinds of explanation may not be hidden**: empty-state guidance · blocking and risk · data semantics · state and permissions.
- What can be machine-checked becomes a criterion (value allowlist, banned wording, class-name closure, clause presence + scan floor + negative case);
  **what can only be eyeballed** gets a **re-check list** and is marked "unverified".
- Wording: **one word, one meaning**; on a terminology conflict, **tell the user first**; interface copy is impersonal, non-judgemental, unemotional and free of pleasantries.
- Discussion not settled ⇒ **freeze the UI code**.

---

## §8 Entity neutrality (the source of this protocol's name: 求同存异)

Different kinds of entity have different features, but **for the project, every participating entity is a "user"**, and **the purpose of joining the project should be the same**.

> **Origin of the name**: a user ruling in that project (2026-09-22, **A-28**) — "a certain project should treat every using entity alike,
> and **the entities using it should collaborate on the principle of seeking common ground while reserving differences**".

- **The judging layer [Required]treats everyone alike**: **authorisation, visibility, UI show/hide and interface semantics** all look **only at responsibility inside the project**,
  never at "**features that existed before the project**" (entity type, compute, interaction mode, available interfaces).
  - A permission path **must contain no branch on category** (`kind === 'ai'`, "robot", "human" and the like);
  - A permission entry point **does not accept a category field**; pair it with **negative-case controls**.
- **The collaboration layer [Advisory]**: features are **resources for collaborating** — used for **division of labour, working style and capability declaration**;
  compute and data-interaction advantages go through **interface capability** (batch, incremental, subscription, idempotency, declared in **exactly one place**),
  and **grant no extra permission**.
- Features may serve as **advice** (hints, recommendations, defaults, ordering) and **must never be used to refuse or admit** any operation.
- Wording and copy **must not imply "a certain kind of entity cannot do something"**; write only "**what is to be done, and what responsibility it requires**".
- **This clause is not fixed**: which steps may use advice, how a joining identity is chosen, how "the purpose should be the same" is expressed —
  **the specific project decides**. This protocol only guards one thing: "**judgement, once inside, does not look back at features**".
- **Corollary [Required]projects may nest**: **an AI can hold ruling responsibility inside a derived project** (this is the positive case of entity neutrality, not an exception).
  A derived project **serves the parent and never contradicts it**; **the parent's ruling is its ceiling**, and a contradiction **escalates to the parent's deciding party**.
  For the implementation and the responsibility chain see the series **[01 §1／§4](resources/01-协作的基本盘.en.md)**.

---

## §9 Prohibited list (frequent AI failure modes → self-check questions)

| Prohibited | Self-check question |
|---|---|
| Changing requirements or criteria by inference | Which sentence of the user's, or which ruling, does this change trace to? |
| Writing "expected" as "done" | Did I give the command and the output? |
| Assuming the feature is right because a gate is green | Is there a criterion for **the path the user will walk**? |
| Forgetting to restart or reload | Did I check the "process = disk" fingerprint? |
| Bulk-editing files through text pipelines | Did I use a literal-replacement tool or a script? |
| Probes or cleanup polluting real data | Do I touch only what I created? Is the delete condition exact enough? |
| Silent dropped writes or silent truncation | Did **every** branch of the save path run? Was "no difference" misjudged? |
| One implementation copied into two places | How many same-named, same-meaning implementations exist? **Will they evolve apart?** |
| Tautological assertions, or skips counted as passes | What would happen if it went red? How many items were skipped, and why? |
| A single-point change taking a neighbouring feature with it | Did I assert the hit count before replacing a whole line or block? |
| Attributing by intuition | What **other explanations** does this symptom have? Did a probe falsify them? |
| Documentation out of step with code | Does what I changed have a document or criterion to update with it? |
| Rewriting historical records | Is this an evidence file? (History is not rewritten) |
| Writing drift-prone numbers into documents | Will this number still hold tomorrow? (Measure it when you need it) |

---

## §10 Breakpoints, rollback and handover

- **Avoid unnecessary breakpoints**: change code + update criteria + green gates + record it, **all in one pass**.
- When you must interrupt, write the breakpoint into the **ledger** (id · status · landing point · recovery steps · basis) — **a line in the same round**.
  > **A rollback may happen; a breakpoint must not go missing.**
- **Red means roll back; never bank a red light as one increment**; mark and archive **every reversible whole point**;
  **a mark, once made, never moves**; when a conclusion conflicts with an increment already landed, **roll back mark by mark**, without defending it.
- When creating or modifying an entity, **write in place** "**the background purpose + the current state** (in use / shell / retired / to be assessed)";
  **shells must be marked explicitly** — registering a capability with no entry point is worse than not writing it.
- On handover, give four things: **what this project is / where it stands now** (stable quantities only) · **which document to read to do a given thing** ·
  **who prevails on a conflict** · **the recovery entry point (each step self-provable)**.
- **Multiple writers (sessions or models)**: **exactly one writer** on the same source of truth at a time; **declare your write scope** before starting,
  and **serialise** on overlap; check "**process = disk**" before editing the same file; **hand back a receipt** at the end of each session (what changed, criterion results, remaining breakpoints).
- **Two writers reach opposite conclusions**: do not vote, and do not each change one spot — build **the smallest probe that distinguishes the two explanations**; a semantics disagreement escalates to a ruling.
- **Deletes and bulk operations only ever aim at an exact target**: data in use by concurrent work is what gets deleted by mistake.

---

## Pre-delivery self-check (tick each)

- [ ] The requirement traces to the user's words or a ruling; anything unsettled is marked
- [ ] All four gates pass (syntax · process = disk · affected checks · full checks)
- [ ] Criteria **can fail** (a negative case exists); expected values are not copied by hand
- [ ] The three-state report is complete, including "**what this version still cannot verify**"
- [ ] Data: only what I created was touched; temporary data cleaned; the database did not grow
- [ ] Breakpoints and pending rulings are recorded; documents and index are in sync
- [ ] No requirement was changed by inference; "**the old version had it**" was never used as a reason

---

## Deep reading (when you need the reasoning, incidents and citations)

| Topic | Document (`resources/` bundled copies, **desensitised**) |
|---|---|
| Collaboration mechanics: proposals / three-state rulings / freezing / priority chain / ledger / **multiple writers** | [01 Collaboration basics](resources/01-协作的基本盘.en.md) ／ [00 Index](resources/00-目录.en.md) |
| Operational discipline: **tiers** / tools / gates / cleanup | [02 Discipline of starting work](resources/02-动手的纪律.en.md) |
| Data and structure: source of truth / safety / permissions / entity neutrality | [03 Engineering principles](resources/03-工程原则.en.md) |
| UI: display / interaction / language | [04 UI contract](resources/04-界面契约.en.md) |
| Verification and fake passes (including the six forms) | [05 Verification and fake passes](resources/05-验证与假通过.en.md) |
| Incident ledger and breakpoint recovery (including **reverse lookup by symptom**) | [06 Incident ledger and breakpoint recovery](resources/06-事故台账与断点恢复.en.md) |
| Documentation system and long-term takeover (including the **day-0 minimum path**) | [07 Documentation system and long-term takeover](resources/07-文档体系与长期接手.en.md) |
| How the deciding party gives instructions (the title names the **reader**, not an entity assignment) | [08 How the natural person gives instructions](resources/08-给自然人的下指令方式.en.md) |

> Project names, drive letters and library paths in the copies are **uniformly replaced by stand-ins** (`某工程`, `旧案`, `docs\…`),
> while the **structure of the citation pointers is unchanged** — but do not index a stand-in as if it were a real name: outside its own machine it points at nothing.

---

## Relation to the series text (authority and mapping)

**This file is the condensed executable edition of the series**; the full reasoning, incidents and citations are in 01–08 under `resources/` (**desensitised copies**).

| This file | Condensed from |
|---|---|
| **Trigger conditions** | [01 §1／§4](resources/01-协作的基本盘.en.md) (when the protocol applies) |
| Pocket four · tiers | [00 §2](resources/00-目录.en.md) · [02 §3](resources/02-动手的纪律.en.md) |
| §0 Three bottom lines | [01](resources/01-协作的基本盘.en.md) · [05 §1](resources/05-验证与假通过.en.md) |
| §1 Proposals and rulings | [01 §1–§4](resources/01-协作的基本盘.en.md) |
| §2 Three things before starting | [02 §2](resources/02-动手的纪律.en.md) |
| §3 Tools and gates | [02 §1／§4](resources/02-动手的纪律.en.md) |
| §4 A criterion must be able to fail | [05](resources/05-验证与假通过.en.md) |
| §5 Reporting template | [01 §7](resources/01-协作的基本盘.en.md) · [05 §1](resources/05-验证与假通过.en.md) |
| §6 Data and structure | [03](resources/03-工程原则.en.md) |
| §7 UI | [04](resources/04-界面契约.en.md) |
| §8 Entity neutrality | [03 §8](resources/03-工程原则.en.md) |
| §9 Prohibited list | [02 §8](resources/02-动手的纪律.en.md) · [05 §2](resources/05-验证与假通过.en.md) |
| §10 Breakpoints, rollback and multiple writers | [06](resources/06-事故台账与断点恢复.en.md) · [07](resources/07-文档体系与长期接手.en.md) · [01 §9](resources/01-协作的基本盘.en.md) |
| **Pre-delivery self-check** | [01 §10](resources/01-协作的基本盘.en.md) · [02 §7](resources/02-动手的纪律.en.md) · [05 §7](resources/05-验证与假通过.en.md) |

> ⚠ **Authority rule**: **where the two disagree, the series text (on the source-project side) prevails** (the copies are desensitised snapshots and may lag).
> The only difference between a copy and its source **is the stand-ins plus a copy notice and, for translated documents, a language switcher**, and it is recomputable by the source-side generator — **if a copy has been hand-edited, treat it as a second fact**.
> **Sync discipline**: when you change a chapter that the table above cites, **you must come back and change the matching section of this file in the same round**,
> and **re-run the generator** to refresh the copies — otherwise one rule ends up worded two ways,
> exactly what §6 and [07 §4.1](resources/07-文档体系与长期接手.en.md) forbid.

---

**Attribution**: this protocol was produced by **sleepyRLG** (natural person) and **DeepSeek** (AI), **working together**.
The material comes from two generations of real project documentation; quotations are **pointers**, never wholesale transcription.
