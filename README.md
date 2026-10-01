# Writing Skills

A set of Claude Code slash commands for prose writing projects: novels, story collections, long-form creative nonfiction.

These are human-in-the-loop workflows. Claude generates reference material, critique, and starter passages — the author makes every final decision about what lands in the draft.

---

## Commands

| Command | What it does |
|---|---|
| `/init` | Scans your project, asks a few questions, and writes the `WRITING.md` config file that all other commands depend on. Run this first. |
| `/critique <draft>` | Reviews a draft and appends a new dated critique section to a `- Critique.md` file. Cross-references factual claims against your reference documents and beat plan. |
| `/critique-help <draft>` | Works through the open annotations in a critique file, phase by phase: grammar fixes, scaffolding review, inline walkthrough, continuity conflicts, summary suggestions. |
| `/chapter-start <chapter>` | Writes a complete reference draft of a chapter as `<!-- WRITING BLOCK -->` comment blocks. The author writes their own version from these and deletes the blocks when done. |
| `/writing-block <chapter>` | Compares a draft against its beat plan and produces a coverage map. Generates starter passages as comment blocks for missing and partial beats. Also has a `read-through` mode for working from read-through log observations. |
| `/read-through <draft>` | Reads the draft as a first-time reader without consulting any plan or reference doc. Reports momentum stalls, clarity issues, reader wants, and the ending as a reading experience. |
| `/epigraph <chapter>` | Generates 2–3 candidate epigraphs in the correct tradition voice. The author picks one and places it as a comment block at the top of the draft. |
| `/continuity-check` | Reads all drafts in narrative order and audits for cross-chapter contradictions and inconsistencies with the reference documents. Outputs a tracked checkbox log. |
| `/grill-me` | Interviews you about a plan or decision, walking the design tree one question at a time until shared understanding is reached. |
| `/gdoc-suggest <doc url>` | Live-document counterpart to `/critique` + `/critique-help`. Reviews a Google Doc and leaves feedback directly in it as real Google Docs suggestions: concrete fixes as suggested text replacements, structural/judgment-call notes as suggested `[Note: ...]` insertions. Nothing is written to the manuscript outside of Suggesting mode. |
| `/gdoc-flow-pass <doc url> <pages>` | Reads a page range of a Google Doc for sentence and paragraph flow (comma splices, loose pronouns, repeated words, dialogue tags, telling labels, clumsy transitions) and leaves each fix as a tracked suggestion, one verified edit at a time. |

---

## Install

1. Copy the `.claude/commands/` folder from this repo into the root of your writing project.
2. Run `/init` — it will scan your project, ask a few questions, and write `WRITING.md` for you.
3. Run any other command from your project directory.

Alternatively, copy `WRITING.md.example` to `WRITING.md` and fill it in manually.

That's it. The commands read `WRITING.md` at startup to discover your project structure.

---

## Project config: WRITING.md

Every project needs a `WRITING.md` at its root. This file tells the commands where your drafts live, which files are reference documents, and where your planning files are.

Copy `WRITING.md.example` to `WRITING.md` in your project and fill it in. The fields are:

**Drafts section:** the directory where draft files live, the naming pattern, and the suffixes used for critique and read-through log files.

**Reference Documents section:** a list of files that are the canon / source of truth for your project. `/critique` and `/continuity-check` will read all of these when checking for conflicts. Include a short description of each so the AI knows what each file covers.

**Story Structure section:** optional paths to a chapter plan and a beat plan. If you don't have these, the commands that depend on them will ask or skip gracefully.

**Epigraph Traditions section:** optional. If defined, `/epigraph` will use these to select and generate candidates. If absent, it will ask you to describe the voice before generating.

---

## Scaffolding types

The commands use two kinds of comment blocks as scaffolding inside draft files:

- `<!-- WRITING BLOCK: [label] -->` ... `<!-- END WRITING BLOCK -->` — starter passages and reference drafts awaiting the author's version. Block-level, wrapped in HTML comments, invisible in Obsidian reading mode.
- `%% SUGGESTION [label]: [text] %%` — inline alternatives and depth suggestions. Obsidian-style comments, invisible in reading mode, visible in edit mode.

Neither type lands directly in the draft as final prose. The author writes their own version and deletes the block when done.

`/gdoc-suggest` is the exception: it targets a live Google Doc instead of a markdown file, so it uses Google Docs' own Suggesting mode rather than comment-block scaffolding. Concrete fixes become real suggested text replacements; structural or judgment-call feedback becomes a suggested `[Note: ...]` insertion. Both are accepted or rejected directly in the doc, no manual cleanup pass required.

---

## Authorship guardrail

AI-generated prose never enters a draft as final text. Commands generate text as reference and inspiration only, placed in comment scaffolding. The author writes the draft; Claude helps them see it more clearly and get unstuck.

---

## Git workflow

Every command that saves changes commits and pushes to the current branch. If the `CLAUDE_CODE_ENTRYPOINT` environment variable is `cli` (local Claude Code), no pull request is created. If it is anything else (a cloud instance), a pull request into `main` is opened via GitHub MCP tools.
