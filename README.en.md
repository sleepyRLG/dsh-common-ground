# Common Ground · 求同存异

**One aim, many natures.**
A collaboration protocol for a natural person and AI entities working on the same long-running
project — rules written to be **executed**, not admired.

`skills/dsh-common-ground/SKILL.md` is the protocol (agent-executable). `resources/` holds nine
chapters of the reasoning, the citations and the incident records behind it.

> **Language**: the [Chinese README](README.md) is **authoritative**; this file is a translation.
> If they disagree, the Chinese one prevails.
> **Name**: `Common Ground` is an English name of the **same register** as 求同存异 — a fixed,
> concept-level expression, not a functional description. Skill slug: `dsh-common-ground`;
> the pinyin alias `qiutongcunyi` is kept in the skill `description` for Chinese-language search.

---

## In three lines

- **Human** working with an AI on a project that outlives one session: this gives you a shared
  vocabulary for rulings, verification and handover.
- **AI agent**: `SKILL.md` says what to do *before* touching code, data, schema, permissions or UI;
  how to report results without fake passes; how to resume after a broken file.
- **Building a skill/agent framework**: the format is a directory with `SKILL.md` plus
  `name`/`description` frontmatter — the convention most loaders already scan.

## The problem

Long projects rarely fail for lack of intelligence. They fail for:

- **Fake passes** — a check never seen to fail; a "verified" claim nobody actually ran.
- **Declaration vs implementation drift** — a guard registered but never enforced.
- **Judgment criteria quietly loosened** to make the current run green.
- **Stale numbers** — written down once, wrong from that moment on.
- **Authority that survives revocation** — permissions snapshotted at login and kept alive.
- **One word, two referents** — the same term meaning different things in two files, found late
  and corrected with a full rework.
- **Lost breakpoints** — nobody knows what was mid-flight when the session ended.

## Install

### DeepSeek Harness

```bash
git clone --depth 1 https://github.com/<you>/dsh-common-ground
mkdir -p ~/.agents/skills
cp -r dsh-common-ground/skills/dsh-common-ground ~/.agents/skills/
```

```powershell
git clone --depth 1 https://github.com/<you>/dsh-common-ground
New-Item -ItemType Directory -Force "$env:USERPROFILE\.agents\skills" | Out-Null
Copy-Item -Recurse .\dsh-common-ground\skills\dsh-common-ground "$env:USERPROFILE\.agents\skills\"
```

Start a new session; the skill appears in the catalog as `dsh-common-ground`.
`resources/` is optional depth — `SKILL.md` is self-contained.

### Other loaders

Copy `skills/dsh-common-ground/` wherever your loader scans for skills. The frontmatter is
`name` + `description`; nothing else is required.
**Compatibility with loaders other than DeepSeek Harness has not been tested** — see
"What is *not* guaranteed" below.

## The load-bearing rules

1. **Three states, no fourth.** Every result is *verified / unverified / known-unsolved*.
   Writing an unverified claim in verified language is the failure this rule exists for.
2. **A criterion must be able to fail.** Seven hard requirements — including that it must have
   been seen **red** at least once.
3. **Six forms of fake pass**, each named with its recognisable symptom.
4. **False red is a failure too.** A criterion that cries wolf gets switched off, and then
   nothing is checked. So both directions must be demonstrated.
5. **Tiers: light / medium / heavy**, decided by **worst-case consequence**, not by effort.
   Nothing is skipped in any tier — only the depth changes.
6. **One source of truth per fact**; a new read path must answer "**who must *not* see this**".
7. **A permission chain with eight invariants** — deny-wins, monotonic narrowing, deny responses
   that do not leak existence, derived rights never persisted… **the number of layers is your
   project's decision.**
8. **Entity neutrality.** What an entity *is* may advise; it must never judge. Purpose is shared,
   natures differ.
9. **Incident ledger + resumable breakpoints** — what broke, what it cost, how to resume.
10. **Multi-writer discipline** — disjoint write scopes, one reviewer, one final diff.

## Why this is not just a prompt dump

- Every rule is labelled **verified / unverified / known-unsolved** — including the ones that came
  from inference rather than from a real incident.
- The verification harness ships **five checks**, and **each one documents what it does NOT cover**.
  A check that cannot state its blind spots is treated as unfinished.
- Checks are **double-sided**: shown to go **red** on a known-bad input *and* **green** on a
  known-good one. One-sided evidence is explicitly called half a check.
- **Real false-red specimens** from the authors' own machine are recorded — e.g. a shell that
  decoded UTF-8 as the local legacy codepage and reported 110 lines for a 199-line file.

## What is *not* guaranteed (read before adopting)

- **Citations**: only **53 of ~124** source citations were spot-checked; section-level anchors are
  **not** machine-checked.
- **Skill loading**: observed only on the authors' machine. **Other machines and loaders are untested.**
- **Desensitisation**: verified only up to a hand-written forbidden-word list. It does **not** claim
  "all identifying detail is gone".
- **The copies**: `resources/` is a **distribution copy** generated by a fixed rule set from a
  **private source project**. The generator is **not shipped** (it contains the real names by
  definition), so an outside reader **cannot recompute** the copies — the "reproducible copies"
  guarantee **holds only for the authors**. **Treat this repository as the distribution.**
- **Field experience**: distilled from **one** long project; **not yet tested on a second one**.

## Layout

```
dsh-common-ground/                     ← repo root
├── README.md                          ← Chinese front page (authoritative)
├── README.en.md                       ← this file (translation)
├── LICENSE-MIT                        ← license: SKILL.md and scripts
├── LICENSE-CC-BY-4.0                  ← license: all other prose
└── skills/
    └── dsh-common-ground/             ← the skill package (reproducible from its source)
        ├── SKILL.md                   ← the protocol
        ├── SKILL.en.md                ← English translation (sibling pair with SKILL.md)
        ├── SKILL.i18n.yaml            ← consistency record for that pair (content hashes)
        ├── TRANSLATION.md             ← translation contract and glossary (bilingual by construction)
        ├── README.md                  ← internal: authority model, five checks, review channel
        └── resources/                 ← nine chapters (00 index … 08)
            ├── 00-目录.md / .en.md / .i18n.yaml
            └── 01–08 (Chinese; English pending contributions)
```

## Language

**Chinese is the source, English is a translation**: changes land in Chinese first and English follows
in the same round; **on conflict the Chinese side prevails**. See the skill's `TRANSLATION.md`.

- **Translated**: the skill's `SKILL.md` (the protocol) and **all nine** `resources/` documents
  (`00` index + chapters `01`–`08`) — shipped as **sibling pairs in the same directory**
  (`<stem>.md` + `<stem>.en.md` + `<stem>.i18n.yaml`), with a language switcher after the H1 on both
  sides. **No locale directories, no mixed-language files.**
- **Proofreading welcome**: the conventions and the **glossary** (exactly one English term per term,
  with banned aliases) are in the skill's `TRANSLATION.md`. After changing a translation, update both
  hashes in the matching `*.i18n.yaml` — otherwise the consistency gate goes red, **on purpose**.
- **What is machine-checked**: completeness, hash records, language switcher, a **structural signature**
  (heading depths, list item counts, table rows/columns, fences, link counts), link locale, and
  Chinese residue on the English side. **It cannot check whether a translation says the same thing**
  — that takes a human read, and **the English side has not yet had an independent read against the
  Chinese**; the repository records that as unverified rather than claiming it.

## Status & contributing

**v0.1** — the rules have passed machine checks and one round of **independent review**;
anchor-level citation checks are ongoing. Issues and counter-examples are welcome, and the most
valuable contribution is **a rule that failed in practice**, together with what it cost.

> This repository carries **no CI/test badge** — there is no CI, so there is no green to report.
> The project's own rule: **a check that reports "clean" must first be shown to report "dirty"**.

## License & attribution

**Dual-licensed**: `SKILL.md` and scripts are **MIT**; all other prose (this file, `README.md`,
the skill's internal `README.md`, `resources/*.md`) is **CC BY 4.0**. See `LICENSE-MIT` and
`LICENSE-CC-BY-4.0`.

Written by **sleepyRLG** (natural person) × **DeepSeek** (AI), **collaboratively** — the human set
direction, rulings and acceptance criteria; the AI distilled, drafted and verified. The name comes
from a ruling that every participating entity should be treated alike and collaborate by
**seeking common ground while reserving differences**.
