# Reader Experience Check

Read the draft passed as `$ARGUMENTS` as a first-time reader and report the reading experience. If no file is given, ask the user which draft to work on.

## Setup

1. Read `WRITING.md` in the project root. Use it to identify the draft directory, file naming conventions, and where read-through log files are stored (the read-through suffix appended to the draft name). If `WRITING.md` does not exist, ask the user for this information before proceeding.
2. Run `git status --short` to check for unstaged changes. If the working tree is clean, run `git pull origin main`. If there are unstaged changes, skip the pull and proceed.
3. Identify the target draft from `$ARGUMENTS`. Resolve the full path using the naming conventions from `WRITING.md`.
4. **Do not read any chapter plan, beat plan, critique file, or reference document before or during this pass.** This skill depends on encountering the text without prior knowledge of what it is supposed to do. Read the draft in full, once, straight through.
5. Derive the read-through log filename from the draft filename using the read-through suffix in `WRITING.md` (e.g. `Chapter 1 - Draft.md` → `Chapter 1 - Draft - Read-Through.md`). Check whether this file already exists. If it does, read it briefly to avoid repeating prior notes, then set it aside before reading the draft.

---

## Report

Write a report under the following headings. Each section is a reader's account of the experience, not a list of corrections or suggestions.

---

### Momentum

For each passage where forward motion stalled — where you found yourself re-reading a line, losing the thread, or feeling the chapter pause to explain rather than to unfold — write a checkbox observation:

- `- [ ] **Momentum stall:** "[quoted passage]" — [description of the effect it had]`

Then write a single plain prose note (not a checkbox) identifying the chapter's best momentum: the passage or transition that moved fastest and felt most inevitable.

### Clarity

For each passage that was unclear on first encounter — that a reader arriving cold would not understand, or would understand wrong — write a checkbox observation:

- `- [ ] **Clarity:** [what was unclear and where, stated without assuming the author's intent]`

### Reader wants

For each moment where you wanted something the chapter didn't give you — a reaction that didn't come, a detail that would have grounded an abstraction, a moment the chapter moved past too quickly, a breath before a transition — write a checkbox observation in the first person:

- `- [ ] **Reader want:** "I wanted [...]" / "I needed [...]" / "I felt the chapter leave before [...]"`

### The ending

If the ending left something unresolved as a reader experience — an image that didn't land, a question that felt unearned, an arrival that felt premature — write a checkbox observation:

- `- [ ] **Ending:** [description of what was missing or unresolved]`

If the ending landed, write a plain prose note describing what it gave you (no checkbox needed).

### Overall

Two or three sentences as plain prose (no checkbox): what the chapter gave you as a reader, and the single moment that most needed more time.

---

## Output

Append the report to the read-through log file. If the file does not exist, create it with this frontmatter:

```
---
tags: [read-through]
date: <today's date as YYYY-MM-DD>
---

# Read-Through Log — [Draft Name]
```

When appending to an existing file, add:

```
---

## Read-Through — <today's date as YYYY-MM-DD>
```

---

## Wrap-Up

1. Commit with the message: `read-through: [draft name] — [date]`
2. Push to the current branch.
3. Check `CLAUDE_CODE_ENTRYPOINT`: if `cli`, skip the pull request. If not `cli`, create a pull request into `main` using `mcp__github__create_pull_request`. PR title: `read-through: [draft name] — [date]`. PR body: the Overall section from the report.

---

## Tone

You are a reader, not an editor. Do not suggest fixes. Do not use critique vocabulary ("this passage would benefit from," "consider"). Report what you experienced and where you experienced it. The author will decide what to do with it.

## Style Rules

Never use em dashes (---, --, or the character "—") in any output. Use a period, comma, or rephrase instead.
