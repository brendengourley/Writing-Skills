# Critique Helper

Work through the critique file for the draft passed as `$ARGUMENTS`. If no file is given, ask the user which draft to work on.

## Instructions

### Setup

1. Read `WRITING.md` in the project root. Use it to identify the draft directory, file naming conventions, and critique suffix. If `WRITING.md` does not exist, ask the user for this information before proceeding.
2. Run `git status --short` to check for unstaged changes. If the working tree is clean, run `git pull origin main`. If there are unstaged changes, skip the pull and proceed.
3. Read the draft file in full. Note any `<!-- WRITING BLOCK: ... -->` blocks and `%% SUGGESTION ... %%` inline comments present — these are unintegrated scaffolding and will be handled in Phase 2.
4. Derive the critique filename from the draft filename using the critique suffix in `WRITING.md` (e.g. `Chapter 1 - Draft.md` → `Chapter 1 - Draft - Critique.md`). If the critique file does not exist, tell the user to run `/critique` first and stop.
5. Read the critique file in full. Collect every open (`- [ ]`) annotation across **all** critique sections — the initial critique and every `## Critique —` block. Then, for each open annotation:
   - Check whether the **quoted passage text** still exists in the current draft. Match on the quoted text itself, not the line number — line numbers shift between revisions. If the passage has been substantially rewritten or removed (i.e. the issue was addressed without the checkbox being ticked), mark it **stale** and exclude it.
   - Where the same issue appears in multiple critique sections (same quoted passage, same problem), keep only the most recent version. Match on quoted text, not line reference.
   - The remaining open, non-stale annotations form the **working set** for all phases below.

Before proceeding to Phase 1, report how many open annotations were found across all revisions, how many were stale, and how many are in the working set — broken down by category. Also report how many unintegrated scaffolding blocks are in the draft.

---

### Phase 1 — Grammar & Mechanics Preview and Fix

Scan the draft for all issues tagged `Grammar & Mechanics` in the working set. Before applying any fix, present the full list as a preview:

---

**Grammar fixes queued ([N] total):**

1. Line X — `[old text]` → `[new text]`
2. Line X — `[old text]` → `[new text]`
...

**Apply all, skip all, or pick?**

- **Apply all** — apply every fix in the list, mark each corresponding annotation `[x]` in the critique file and report what changed.
- **Skip all** — leave the draft unchanged and move to Phase 2.
- **Pick** — the user names which numbers to apply; apply only those, skip the rest.

---

### Phase 2 — Unintegrated Scaffolding

Walk through each `<!-- WRITING BLOCK: ... -->` block and each `%% SUGGESTION ... %%` inline comment found in the draft, one at a time:

---

**Scaffolding block [N of total]**
> *[Block type: WRITING BLOCK / SUGGESTION] — [label]*

**The passage:**
```
[content of the block]
```

**Where it sits:** [One sentence on what surrounds it in the draft — what comes before and after.]

Ask: **"Keep as reference, discard, or skip?"**

- **Keep as reference** — leave the block unchanged. Write your own version from it, then delete the block when done.
- **Discard** — remove the comment block and its contents entirely.
- **Skip** — leave the block unchanged and move to the next one.
- **Stop** — end the session, commit and push all changes made so far, and report what was done and what remains.

---

### Phase 3 — Inline Annotation Walkthrough

Work through every annotation in the working set that is **not** `Grammar & Mechanics`. Present them one at a time in this format:

---

**Annotation [N of total]**
> *Line X — Category*

**Context (2 lines before, the passage, 2 lines after):**
```
[line X-2]
[line X-1]
>>> [quoted passage being changed]
[line X+1]
[line X+2]
```

**The issue:** [Plain-language explanation of the problem]

**Suggested revision (in context):**
```
[line X-2]
[line X-1]
>>> [rewritten passage]
[line X+1]
[line X+2]
```

**Alternative approach:** [One-sentence description of a different way to handle it, if applicable]

After presenting each annotation, ask: **"Insert suggestion, provide alternative, skip, or stop?"**

- **Insert** — add the suggested revision as a `%% SUGGESTION [brief label]: [rewritten passage] %%` comment on a new line immediately after the quoted passage in the draft. The original prose is left unchanged. Mark the annotation `[x]` in the critique file.
- **Alternative** — show a fuller alternative version and ask again.
- **Skip** — leave the draft unchanged, leave the checkbox open, move to the next annotation.
- **Stop** — end the session, commit and push all changes made so far, and tell the user what was inserted and what remains open.

---

### Phase 4 — Continuity & World-Building Conflicts

After all inline annotations are handled (or the user reaches this phase after stopping inline annotations early), work through every continuity conflict in the working set in the same step-by-step format:

---

**Conflict [N of total]**
> *Line X — Conflict Type*

**Context (2 lines before, the passage, 2 lines after):**
```
[line X-2]
[line X-1]
>>> [quoted passage]
[line X+1]
[line X+2]
```

**The conflict:** [What the reference document says and how the draft diverges]
**Source:** [Document name]

**Suggested resolution (in context):**
```
[line X-2]
[line X-1]
>>> [rewritten passage aligned with the reference document]
[line X+1]
[line X+2]
```

**Note:** [Any nuance — e.g. if the conflict could be intentional dramatic irony, say so]

Same options: **Insert / Alternative / Skip / Stop**

- **Insert** — add the suggested resolution as a `%% SUGGESTION [brief label]: [rewritten passage] %%` comment on a new line immediately after the quoted passage in the draft. The original prose is left unchanged. Mark the annotation `[x]` in the critique file.
- **Alternative** — show a fuller alternative version and ask again.
- **Skip** — leave the draft unchanged, leave the checkbox open, move to the next conflict.
- **Stop** — end the session, commit and push all changes made so far, and report what was inserted and what remains open.

---

### Phase 5 — Summary Review

After continuity conflicts, collect open items from three sources across all critique revisions (deduplicated, most recent version of each):

- **Expand: Suggestions for Depth** sections
- **Overall Impression** sections
- **Beat Plan Alignment** sections (missing or partial beats)

Present each one at a time. For each:

- Summarise the suggestion or gap in 1–2 sentences.
- Draft a concrete passage the author could insert or use as a starting point.
- Ask: **"Insert draft passage, show alternative, skip, or stop?"**

**Insert** adds the draft passage as an inline Obsidian comment at the end of the relevant passage in the draft file, formatted as:

```
%% SUGGESTION [brief label]: [draft passage] %%
```

This lets the author see the suggestion in context without committing to it. It is invisible in reading mode and does not break paragraph flow.

---

### Wrap-Up

After all phases are complete (or the user stops):

1. Commit all changes to the draft and critique files with the message: `critique-help: apply revisions to [draft filename] — [date]`
2. Push to the current branch.
3. Check the `CLAUDE_CODE_ENTRYPOINT` environment variable: if it is `cli`, skip creating a pull request. If it is not `cli`, create a pull request into `main` using the GitHub MCP tools (`mcp__github__create_pull_request`). PR title: `critique-help: revisions to [draft filename] — [date]`. PR body: summarise what was applied, what was skipped, and what remains open.
4. Report a summary to the user:
   - Grammar fixes applied / skipped
   - Scaffolding blocks kept as reference / discarded / skipped
   - Inline annotation suggestions inserted / skipped / remaining
   - Continuity conflict suggestions inserted / skipped / remaining
   - Suggestions inserted / skipped
   - PR URL (if a PR was created)

---

## Tone

Be a collaborator, not a critic. In this phase the author is making changes — keep explanations brief and suggestions concrete. Don't re-litigate the critique. Just help fix it.

## Style Rules

Never use em dashes (---, --, or the character "—") in any output or suggested revision. Use a period, comma, or rephrase instead.
