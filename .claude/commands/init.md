# Writing Project Init

Set up `WRITING.md` for this project. Run without arguments.

Scans the project to discover what's already there, asks you to fill in what it can't determine, then writes the config file that all other writing commands depend on.

## Instructions

### Step 1 — Check for existing config

Check whether `WRITING.md` already exists in the project root.

- If it exists, read it and tell the user what it currently contains. Ask: **"Update the existing WRITING.md, or stop?"**
  - **Update** — proceed through the steps below, pre-filling answers from the existing file where possible, and replace the file at the end.
  - **Stop** — end without changes.
- If it does not exist, proceed.

---

### Step 2 — Discover project structure

Scan the project to gather evidence before asking the user anything. Run these checks:

1. **Project title** — look for a title in any existing README, a top-level .md file, or a git remote URL. Note what you find; don't assume.
2. **Draft directory** — list all directories in the project root. Identify any that contain .md files with sequential naming (e.g. "Chapter 1", "Part 1", numbered filenames). Note the most likely candidate and its naming pattern.
3. **Reference documents** — list all .md files in the project root (not in subdirectories). For each, read the first 5–10 lines to assess whether it looks like worldbuilding, character notes, a magic system, a style guide, or other reference material vs. a plan or a draft. Note the likely reference docs.
4. **Story structure files** — among the root .md files, identify any that look like a chapter plan (one entry per chapter with POV/summary) or a beat plan (beat-by-beat breakdowns per chapter or scene).
5. **Existing draft filenames** — if a draft directory was found, list a few filenames from it to determine the exact naming pattern (e.g. `Chapter 1 - Draft.md`, `01-opening.md`, `Part 1 Scene 2.md`).
6. **Critique and read-through files** — look for files in the draft directory that follow a pattern suggesting they are critique or read-through logs (e.g. files containing "Critique" or "Read-Through" in the name). Infer the suffixes used.
7. **Continuity log** — look for a file anywhere in the project that looks like a continuity or consistency tracking log.
8. **Epigraph traditions** — look for any document that describes distinct voices, traditions, registers, or styles that could serve as epigraph sources (e.g. a language and culture document, a worldbuilding file with named cultural groups).

Report your findings in a brief summary before proceeding to Step 3. Flag anything you could not determine.

---

### Step 3 — Ask what you couldn't determine

For each piece of information you could not confidently determine from scanning, ask the user. Group related questions together and keep it conversational — don't present a form. Cover:

- Project title (if not found)
- Draft directory and naming pattern (if ambiguous or not found)
- Which root .md files are reference documents vs. planning files vs. something else (show your best guess, ask them to correct it)
- Chapter plan path (if not found or ambiguous)
- Beat plan path (if not found or ambiguous)
- Critique suffix used (or confirm the default: ` - Critique`)
- Read-through suffix used (or confirm the default: ` - Read-Through`)
- Continuity log filename (or confirm the default: `Continuity Check.md`)
- Epigraph traditions: ask whether the project has distinct source traditions for chapter epigraphs. If yes, ask the user to describe each one (name, voice/register, attribution format, thematic territory). If no, skip the Epigraph Traditions section.

If you are confident about something from Step 2, tell the user what you found and ask them to confirm or correct it rather than asking a blank question.

---

### Step 4 — Write WRITING.md

Using everything gathered in Steps 2–3, write `WRITING.md` to the project root using this format:

```
# Writing Project: [Title]

This file is read by the Claude writing commands (`.claude/commands/`) to understand
your project's structure.

---

## Drafts

- **Directory:** `[draft directory]/`
- **Naming pattern:** `[pattern, e.g. Chapter {N} - Draft.md]`
- **Critique suffix:** ` - [suffix]` (appended before `.md`, e.g. `[example filename]`)
- **Read-through suffix:** ` - [suffix]` (appended before `.md`, e.g. `[example filename]`)
- **Continuity log:** `[filename]`

---

## Reference Documents

[List each reference doc with a one-line description of what it covers]

---

## Story Structure

[Include only the lines that apply; omit lines for docs that don't exist]
- **Chapter plan:** `[path]` ([brief description of what it contains])
- **Beat plans:** `[path]` ([brief description of what it contains])

---

## Epigraph Traditions (optional)

[Include this section only if the user confirmed traditions exist. For each:]

### [Tradition Name]
Voice and register: [description]
Attribution format: [description]
Thematic territory: [description]
```

Omit sections or lines that don't apply. Do not include placeholder text or comments in the written file — everything in the file should be real, filled-in information.

Show the user the completed `WRITING.md` before writing it. Ask: **"Write this file, adjust something, or stop?"**

- **Write** — write the file to the project root, then proceed to Step 5.
- **Adjust [something]** — incorporate the change and show the updated version, then ask again.
- **Stop** — end without saving.

---

### Step 5 — Wrap-Up

After writing `WRITING.md`:

1. Commit with the message: `init: add WRITING.md — [date]`
2. Push to the current branch.
3. Check `CLAUDE_CODE_ENTRYPOINT`: if `cli`, skip the pull request. If not `cli`, create a pull request into `main` using `mcp__github__create_pull_request`. PR title: `init: add WRITING.md — [date]`. PR body: a one-sentence description of the project and a summary of what was configured (reference doc count, whether a beat plan exists, whether epigraph traditions are defined).
4. Tell the user: "WRITING.md is set up. You can now run /critique, /writing-block, /chapter-start, /read-through, /epigraph, and /continuity-check."

---

## Tone

Keep the questions brief. The user knows their project — you're just helping them express its structure in a file. Don't over-explain each field. If you're confident about something, say so and let them correct you rather than asking from scratch.
