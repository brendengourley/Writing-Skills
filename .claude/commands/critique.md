# Creative Writing Reviewer

Review the draft passed as `$ARGUMENTS`. If no file is given, ask the user which file to review.

## Instructions

1. Read `WRITING.md` in the project root. Use it to identify: the draft directory and naming conventions, all reference documents (the canon / source of truth for the project), the chapter plan path, and the beat plan path. If `WRITING.md` does not exist, ask the user for this information before proceeding.
2. Run `git status --short` to check for unstaged changes. If the working tree is clean, run `git pull origin main`. If there are unstaged changes, skip the pull and proceed.
3. Read the draft file in full. Treat any `<!-- WRITING BLOCK: ... -->` and `<!-- SUGGESTION: ... -->` comment blocks as pending scaffolding, not final prose — do not annotate them as prose problems. Count how many such blocks are present; you will note this in the summary.
4. Derive the critique filename from the draft filename using the critique suffix in `WRITING.md` (e.g. `Chapter 1 - Draft.md` → `Chapter 1 - Draft - Critique.md`).
5. If the critique file already exists, read it in full.
   - Count the number of existing `## Critique —` sections to determine the revision number for this pass. The first critique has no revision label; the second is `(Rev 2)`, the third is `(Rev 3)`, and so on.
   - Collect every open (`- [ ]`) annotation from all prior sections. For each, check whether the quoted passage still exists in the current draft. If the issue has been addressed or the text rewritten, mark it resolved (do not carry it forward). The remaining open, unresolved annotations are your **carry-over set**.
   - Do not repeat annotations for issues already addressed.
   - Focus new annotations on issues not previously raised, or on regressions introduced in this revision.
6. Read all reference documents listed in `WRITING.md` in full. Cross-reference every factual claim in the draft against these documents — character names, traits, abilities, relationships, place names, world rules, cultural details, technology, history. Note which document was the source of any conflict found.
7. Read the chapter plan and beat plan (paths from `WRITING.md`). Find the entry for the chapter or section being critiqued. Note what it is meant to accomplish — its beats, POV, and narrative purpose. Use this to assess whether the draft is doing the structural work the plan calls for, not just whether it reads well in isolation.
   - If no chapter plan or beat plan exists for this project, skip step 7 and note the absence in the critique.
8. Append the new critique as a new section at the bottom of the critique file. Do not overwrite or remove prior critique sections.
9. After writing the file, commit it and push to the current branch. Then check the `CLAUDE_CODE_ENTRYPOINT` environment variable: if it is `cli`, skip creating a pull request and tell the user the changes were committed and pushed. If it is not `cli`, create a pull request into `main` using the GitHub MCP tools (`mcp__github__create_pull_request`). PR title: `Critique: <draft filename> — <today's date> (Rev N)`. PR body: summarize the number of annotations, continuity conflicts found, and the Overall Impression. Tell the user the PR URL when done.

---

## Output File Format

If creating the file for the first time, begin with this frontmatter:

```
---
tags: [critique]
date: <today's date as YYYY-MM-DD>
---
```

When appending to an existing file, add a markdown horizontal rule (`---`) followed by a dated section header before the new content. Include the revision number if this is not the first critique:

```
---

## Critique — <today's date as YYYY-MM-DD> (Rev N)
```

Then include the following sections:

---

### 0. Revision Summary

Write this section only when appending to an existing critique file (skip it on first critique).

- **Carry-over:** List the count of open annotations still unresolved from prior passes, broken down by category. Do not re-list their full text here — just the count and categories. If all prior annotations are resolved, say so.
- **Scaffolding pending:** Note how many `<!-- WRITING BLOCK -->` and `<!-- SUGGESTION -->` comment blocks remain in the draft unintegrated, if any.
- **This pass:** One sentence on the focus of this critique — new issues, regressions, continuity check, beat plan alignment, or "clean pass, minor items only."

---

### 1. Inline Annotations

Go through the text and flag specific passages. Format each annotation as a checkbox item:

```
- [ ] **Line X – [Category]:** "quoted passage" → Your suggestion.
```

Categories: `Story & Plot` | `Characters` | `Style & Prose` | `Grammar & Mechanics` | `Expand`

The `Expand` category is for paragraphs or moments that are underdeveloped — where more context, detail, or interiority would strengthen the writing. For each `Expand` annotation, write a concrete suggestion of what could be added (e.g. a sensory detail, an emotional reaction, a piece of backstory, a clarifying image). Do not just say "add more detail" — say what kind of detail and why it would help.

Only annotate things worth changing. Aim for actionable, specific suggestions. Do not annotate comment scaffolding blocks.

---

### 2. Continuity & World-Building Conflicts

List every place where the draft contradicts or diverges from the reference documents. Format each as a checkbox item:

```
- [ ] **Line X – [Conflict Type]:** "quoted passage" → Conflicts with [Source Document]: describe what the document says and suggest how to align the draft.
```

Conflict types: `Character` | `World` | `Culture` | `History` | `Technology` | `Other`

If no conflicts are found, write: *No continuity conflicts detected.*

If no reference documents exist for this project, write: *No reference documents configured — skipping continuity check.*

---

### 3. Beat Plan Alignment

Compare the draft against its entry in the chapter plan and beat plan (from `WRITING.md`). For each planned beat, note whether it is **present**, **partial**, or **missing** in the draft. Call out any beat that is missing or only partial and explain what the plan calls for that the draft doesn't yet deliver. If the draft covers all planned beats, say so briefly.

If no beat plan exists for this project or chapter, write: *No beat plan found — skipping beat plan alignment.*

---

### 4. Summary Critique

Write a structured critique with one section per category:

#### Story & Plot
Assess narrative structure, pacing, scene transitions, plot holes, and story arc — including whether the chapter is accomplishing what the beat plan calls for.

#### Characters
Evaluate character voice, consistency, motivation, and development.

#### Style & Prose
Comment on voice, sentence variety, word choice, rhythm, and show-vs-tell balance.

#### Grammar & Mechanics
Summarize recurring mechanical issues. Don't list every instance — describe the pattern.

#### Expand: Suggestions for Depth
List the top 2–3 moments in the piece that most need expansion. For each, write 2–3 sentences describing specifically what could be added and what effect it would have on the reader.

#### Overall Impression
Two to three sentences: what the piece does well, and the single most impactful thing to fix next.

---

## Tone

Be honest and direct, but constructive. Assume the writer wants to improve. Avoid hollow praise. Frame problems as opportunities.

## Style Rules

Never use em dashes (---, --, or the character "—") in any output. Use a period, comma, or rephrase instead.
