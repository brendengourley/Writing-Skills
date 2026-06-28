# Writing Block Helper

Look at the chapter draft and writing plan for `$ARGUMENTS` and surface what is missing, then help get it started. If no chapter is given, ask the user which chapter to work on.

If `$ARGUMENTS` contains the keyword `read-through`, run in **Read-Through Mode** instead: work through the observations in the chapter's read-through log and generate targeted prose responses. See **Read-Through Mode** below.

## Instructions

1. Read `WRITING.md` in the project root. Use it to identify: the draft directory and naming conventions, all reference documents, the chapter plan path, the beat plan path, and the read-through suffix used for log files. If `WRITING.md` does not exist, ask the user for this information before proceeding.
2. Run `git status --short` to check for unstaged changes. If the working tree is clean, run `git pull origin main`. If there are unstaged changes, skip the pull and proceed.
3. Identify the chapter or section from `$ARGUMENTS`.
   - If `$ARGUMENTS` contains the keyword `read-through`, skip steps 4–6 and jump to **Read-Through Mode** below.
   - If `$ARGUMENTS` contains `--session [date]` (e.g. `--session 2026-05-15`), note the date for use in Read-Through Mode.
4. If a chapter plan exists (per `WRITING.md`), read it to find the high-level plan for the target chapter.
5. If a beat plan exists (per `WRITING.md`), read it to find the beat-by-beat plan for the target chapter. If no beat plan exists yet for that chapter, offer to draft one before proceeding (see **Beat Plan Drafting** below). Do not silently fall back to the high-level plan.
6. Read all reference documents listed in `WRITING.md` in full. These are your source of truth for character voice, abilities, world rules, and any other established facts — consult them when writing starter passages to avoid continuity errors.
7. Check whether a draft exists for this chapter. If it does, read it in full and note any `<!-- WRITING BLOCK: ... -->` comment blocks already present — these are unintegrated starter passages from a prior session (status: Scaffolded). If the draft does not exist, treat it as empty.

---

## Beat Plan Drafting

If no beat plan exists for the target chapter, pause before the coverage map and offer:

> "No beat plan exists for this chapter yet. I can draft one from the high-level plan and reference documents, or we can work from the high-level plan directly. Draft a beat plan?"

If the user says yes:
- Using the high-level chapter entry, the reference documents, and any other context available, write a detailed beat-by-beat plan in the same style and format as other entries in the beat plan file (scene headings, bullet-point beats, voice and atmosphere notes).
- Show the drafted plan to the user and ask: **"Approve and save, edit, or work from the high-level plan only?"**
- If approved, append it to the beat plan file under the correct chapter heading, commit with the message `writing-block: add beat plan for [chapter name] — [date]`, and push. Then continue to the coverage map using the new plan.
- If the user wants to edit, incorporate their changes before saving.
- If declined, proceed using the high-level plan only and note this in the coverage map.

---

## Phase 1 — Coverage Map

Compare the draft against the beat-by-beat plan (or high-level plan if applicable). For each planned beat, assess whether it is:

- **Present** — the draft covers this beat in integrated prose
- **Partial** — the draft touches this beat but does not fully develop it
- **Scaffolded** — a `<!-- WRITING BLOCK: ... -->` comment exists for this beat but has not been integrated into the prose
- **Missing** — the draft does not address this beat at all

Present the coverage map in this format:

---

**Coverage: [Chapter/Section Name] — [POV, if applicable]**

| Beat | Status |
|------|--------|
| [Brief beat label] | Present / Partial / Scaffolded / Missing |
| ... | ... |

**Present:** [count] beats
**Partial:** [count] beats
**Scaffolded:** [count] beats (starter passages exist, not yet integrated)
**Missing:** [count] beats

---

If the draft is empty, note that and skip to Phase 2 with all beats marked Missing.

---

## Phase 2 — Gap Walkthrough

Work through each **Missing**, **Partial**, and **Scaffolded** beat one at a time. The format and options differ by status.

---

### Missing beats

**Gap [N of total gaps]**
> *[Beat label] — Missing*

**What the plan calls for:**
[2–3 sentences summarising what this beat needs to accomplish — character, plot, atmosphere, and any specific details from the plan. Note any character traits or world rules from the reference docs that the passage should reflect.]

**Where it fits in the chapter:**
[One sentence on where this beat sits relative to what is already written — before, after, or interwoven with existing content]

**Starter passage:**
```
[A concrete draft passage of 3–8 sentences that the author can use as a starting point, written in the style and voice of the existing draft. This should be prose, not description of prose. Write it as if it belongs in the chapter. Ensure it is consistent with the reference documents.]
```

**What to do with this passage:** [One sentence — e.g. "Insert after the third paragraph", "Replace the current transition at the end of the climbing sequence", "Open the chapter with this before the current first line."]

Ask: **"Insert starter passage, show alternative, skip, or stop?"**

- **Insert** — add the starter passage as a comment block in the draft file at the appropriate location:

```
<!-- WRITING BLOCK: [brief beat label] -->
[starter passage]
<!-- END WRITING BLOCK -->
```

- **Alternative** — write a different starter passage using a different entry point, emotional register, or level of interiority, then ask again.
- **Skip** — leave the draft unchanged, move to the next gap.
- **Stop** — end the session, commit and push all changes made so far, and report what was inserted and what remains.

---

### Partial beats

**Gap [N of total gaps]**
> *[Beat label] — Partial*

**What the plan calls for:**
[2–3 sentences on what this beat is supposed to accomplish in full.]

**What's already there:**
```
[The existing passage from the draft that partially covers this beat]
```

**What's missing:**
[One or two sentences identifying specifically what the plan calls for that the existing prose doesn't deliver — not a general note, but a precise gap.]

**Suggested addition:**
```
[A targeted passage of 1–4 sentences that fills the specific gap identified above. Not a rewrite — an addition that works alongside the existing prose. Ensure it is consistent with the reference documents.]
```

**Where to add it:** [One sentence on where the addition slots in relative to the existing passage — before, after, or within it.]

Ask: **"Insert addition, show alternative, skip, or stop?"**

Same options as Missing beats. Insert wraps the addition in a `<!-- WRITING BLOCK: [beat label] -->` comment block at the specified location — it is not added directly to the prose.

---

### Scaffolded beats

**Gap [N of total gaps]**
> *[Beat label] — Scaffolded*

**Existing starter passage:**
```
[The contents of the WRITING BLOCK comment]
```

**What the plan calls for:**
[One sentence confirming what this beat is supposed to accomplish and whether the scaffolded passage covers it.]

Ask: **"Keep as reference, replace, skip, or stop?"**

- **Keep as reference** — leave the block unchanged. Write your own version from it, then delete the block when done.
- **Replace** — discard the scaffolded passage and write a new starter passage from scratch as a new comment block, then ask Insert / Alternative / Skip.
- **Skip** — leave the block unchanged.
- **Stop** — end the session, commit and push all changes made so far.

---

## Phase 3 — Summary

After all gaps are handled (or the user stops):

1. If any changes were made (starter passages inserted, additions inserted, beat plan saved), commit all changes with the message: `writing-block: [brief description of what changed] — [date]` and push to the current branch. Then check the `CLAUDE_CODE_ENTRYPOINT` environment variable: if it is `cli`, skip creating a pull request. If it is not `cli`, create a pull request into `main` using the GitHub MCP tools (`mcp__github__create_pull_request`). PR title: `writing-block: [brief description] for [draft filename] — [date]`. PR body: list what was inserted, integrated, and skipped.
2. If no changes were made, do not commit, push, or create a pull request.
3. Report a summary to the user:
   - Beats present (count)
   - Beats partial (count)
   - Beats scaffolded (count)
   - Beats missing (count)
   - Starter passages inserted (count)
   - Additions inserted for partial beats (count)
   - Beats skipped (count)
   - PR URL (if a PR was created)

---

---

## Read-Through Mode

**Triggered by:** the keyword `read-through` anywhere in `$ARGUMENTS`.

**Session selection:** By default, work from the most recent session in the log (the last `## Read-Through —` heading). If `$ARGUMENTS` contains `--session [date]` (e.g. `--session 2026-05-15`), find the session whose heading matches that date and use it instead. If the date does not match any session, tell the user which sessions exist and ask which to use.

### Setup

1. Read the draft for this chapter in full.
2. Derive the read-through log filename from the draft filename using the read-through suffix in `WRITING.md`. Check whether this file exists.
   - If **no log exists**: pause and offer: "No read-through log exists for this chapter. I can run a read-through now and then proceed to the walkthrough. Run a read-through?" If yes, follow the `/read-through` skill's full instructions (read the draft without consulting any plan or reference document, produce the report with checkbox observations, append to the log file, commit with `read-through: [chapter name] — [date]`, push), then continue to the walkthrough below. If no, end the session.
   - If **the log exists but has no `- [ ]` checkboxes** (pre-checkbox format): note this to the user and surface the observations as plain-text context during the walkthrough. Do not attempt to mark them resolved.
3. Read the reference documents listed in `WRITING.md`. **Do not read the beat plan or chapter plan — this mode is plan-blind.**
4. Parse all `- [ ]` observations from the selected session. For each, identify which passage in the draft it references so observations can be sorted top-to-bottom by position in the draft.
5. Display the **Overall** note from the selected session as opening context:

> **From the read-through ([date]):** [Overall text]

Then state: "Working through [N] open observations in draft order."

---

### Observation Walkthrough

Work through each open (`- [ ]`) observation one at a time, in the order the referenced passages appear in the draft.

---

**Observation [N of total]**
> *[Category: Momentum stall / Clarity / Reader want / Ending] — [brief label]*

**What the reader noted:**
[The observation text from the log]

**The passage:**
```
[The passage from the draft the observation references, or the nearest relevant passage if no direct quote exists]
```

**Suggested response:**
```
[A targeted prose addition or revision of 2–6 sentences, written in the chapter's voice and consistent with the reference documents. Write prose, not description of prose.]
```

**Where it fits:** [One sentence on where this slots into the chapter — before, after, or within the flagged passage]

Ask: **"Insert, show alternative, skip, or stop?"**

- **Insert** — add the suggested response as a comment block in the draft at the appropriate location, and mark the observation `[x]` in the read-through log:
  ```
  <!-- WRITING BLOCK (read-through): [brief label] -->
  [suggested response]
  <!-- END WRITING BLOCK -->
  ```
- **Alternative** — write a different response using a different angle, emotional register, or level of interiority, then ask again.
- **Skip** — leave the draft and log unchanged. Move to the next observation.
- **Stop** — end the session, commit and push all changes made so far, and report what was inserted and what remains open.

---

### Read-Through Mode Summary

After all observations are handled (or the user stops):

1. If any changes were made (passages inserted, log checkboxes updated), commit all changes with the message: `writing-block: read-through walkthrough for [chapter name] — [date]` and push to the current branch. Then check `CLAUDE_CODE_ENTRYPOINT`: if `cli`, skip creating a pull request. If not `cli`, create a pull request into `main` using the GitHub MCP tools (`mcp__github__create_pull_request`). PR title: `writing-block: read-through walkthrough for [chapter name] — [date]`. PR body: list observations addressed, skipped, and remaining open.
2. If no changes were made, do not commit or push.
3. Report to the user:
   - Observations addressed (count)
   - Observations skipped (count)
   - Observations remaining open (count)
   - PR URL (if created)

---

## Tone

The writer is stuck, not failing. Keep explanations brief. Starter passages should feel like they belong — use the existing chapter's voice, sentence rhythm, and level of interiority. Do not describe what the passage should do; just write it. One good sentence is more useful than three hedged ones.

## Style Rules

Never use em dashes (---, --, or the character "—") in any output or starter passage. Use a period, comma, or rephrase instead.

## Authorship guardrail

AI-generated prose never lands directly in the draft as final text. All generated passages are placed in `<!-- WRITING BLOCK: ... -->` comment blocks. The author writes their own version from these and deletes each block when done.
