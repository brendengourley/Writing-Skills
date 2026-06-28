# Continuity Checker

Audit all existing drafts for cross-draft contradictions and inconsistencies with the project's reference documents. Run without arguments.

## Instructions

1. Read `WRITING.md` in the project root. Use it to identify: the draft directory, all reference documents (the canon / source of truth), the chapter plan path (if any), and the continuity log path. If `WRITING.md` does not exist, ask the user for this information before proceeding.
2. Run `git status --short` to check for unstaged changes. If the working tree is clean, run `git pull origin main`. If there are unstaged changes, skip the pull and proceed.
3. Read all reference documents listed in `WRITING.md` in full.
4. If a chapter plan exists (per `WRITING.md`), read it for the intended narrative sequence, POV assignments, and what each chapter or section is meant to establish.
5. Read every existing draft in the draft directory (per `WRITING.md`) in narrative order. For each, note the chapter or section number, POV, and every concrete fact established — character states, locations visited, objects acquired, abilities used, things said or learned.
6. Check whether a continuity log already exists at the path specified in `WRITING.md`. If it does, read it in full and note which issues are already resolved (`[x]`) — do not re-report those.

---

## Checks to perform

Run all of the following across the drafts and against the reference documents:

- **Characters** — Is each character's voice, psychology, and abilities consistent with the reference documents and with their portrayal across drafts? Look for: contradictions in how abilities are described or named; inconsistencies in what a character knows or believes at a given point; shifts in register that aren't explained by the scene.
- **Timeline** — Do events follow a coherent sequence? Does a character reference something that hasn't happened yet in narrative order? Do chapter transitions feel continuous in time?
- **Geography** — Are locations, directions, distances, and spatial relationships consistent across drafts and with the reference documents? Note any place described differently in two chapters.
- **World rules** — Are any world-specific rules (magic, technology, social structures, cultural practices, etc.) used consistently with the reference documents? Does a character demonstrate an ability before it is established, or apply a rule the reference doesn't support?
- **Cross-draft facts** — Does a fact established in one draft (an object, a relationship, an event, a physical detail) contradict how it appears in another?
- **Naming** — Are character names, place names, group names, and proper nouns spelled and used consistently across all drafts?

---

## Output format

Write or append to the continuity log path from `WRITING.md`.

If creating for the first time:

```
---
tags: [continuity]
date: <today's date as YYYY-MM-DD>
---

# Continuity Check
```

When appending to an existing file, add:

```
---

## Check — <today's date as YYYY-MM-DD>
```

List every issue found as a checkbox item:

```
- [ ] **[Draft A] [vs Draft B] – [Category]:** Description of the contradiction or inconsistency. Source: [reference document or draft].
```

Categories: `Characters` | `Timeline` | `Geography` | `World rules` | `Cross-draft` | `Naming`

For each category, if no issues are found, write a single line: *No [category] conflicts detected.*

Close the section with a short summary paragraph: total issue count, which categories had the most, and the single most important issue to resolve first.

---

## Wrap-Up

1. Commit with the message: `continuity-check: audit — [date]`
2. Push to the current branch.
3. Check `CLAUDE_CODE_ENTRYPOINT`: if `cli`, skip the pull request. If not `cli`, create a pull request into `main` using `mcp__github__create_pull_request`. PR title: `continuity-check: [date]`. PR body: issue count by category and the top-priority issue.

---

## Tone

Report what the text actually says, not what it probably means. A contradiction is worth noting even if it is likely unintentional — flag it and let the author decide. Do not soften findings.

## Style Rules

Never use em dashes (---, --, or the character "—") in any output. Use a period, comma, or rephrase instead.
