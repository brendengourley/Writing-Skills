# Epigraph Writer

Write or refine the epigraph for the chapter or section passed as `$ARGUMENTS`. If no target is given, ask the user which chapter or section to work on.

## Instructions

1. Read `WRITING.md` in the project root. Use it to identify: the draft directory and naming conventions, the chapter plan path (if any), and any epigraph traditions defined in the `## Epigraph Traditions` section. If `WRITING.md` does not exist, ask the user for this information before proceeding.
2. Run `git status --short` to check for unstaged changes. If the working tree is clean, run `git pull origin main`. If there are unstaged changes, skip the pull and proceed.
3. Identify the target chapter or section from `$ARGUMENTS`.
4. If a chapter plan exists (per `WRITING.md`), read it to find the entry for this chapter: POV, themes, and any epigraph tradition already specified.
5. If a beat plan exists (per `WRITING.md`), read the entry for this chapter — the epigraph should resonate with what the chapter does, not summarise it.
6. Check whether a draft exists for this chapter. If it does, read any existing epigraph present so candidates don't simply repeat it.

---

## Phase 1 — Tradition Identification

Identify which tradition or voice the epigraph should come from, using this priority order:

1. **Specified in the chapter plan** — use it.
2. **Defined in `WRITING.md` under `## Epigraph Traditions`** — reason from the chapter's POV and themes to select the most fitting tradition from the list. State your choice and why.
3. **No traditions defined** — ask the user: "What voice or style should the epigraph draw from? Describe the register, thematic territory, any attribution format (author, source text, date), and 1–2 examples of the kind of language you have in mind." Wait for their answer before writing candidates.

State which tradition or style applies and why, before generating candidates.

---

## Phase 2 — Candidates

Write three candidate epigraphs. Each should:

- Sound like it genuinely comes from the stated tradition or style — use the register, vocabulary, and form described.
- Resonate with the chapter's themes without summarising them. The best epigraph lands differently after reading the chapter than before.
- Be brief. No more than 4–5 lines. Epigraphs earn weight through compression, not length.
- Include an attribution line in the style of the tradition (e.g. *Fragment of the Pale Remembrance, speaker and age unknown.* or *Researcher V. Maye, Colonial Records Office, 1842.* or *Traditional, origin disputed.*).

Present them numbered, formatted as they would appear in the draft — italics for the epigraph text, plain text for the attribution.

Ask: **"Use one of these, see alternatives, or stop?"**

- **Use [N]** — place the chosen epigraph as a `<!-- WRITING BLOCK (epigraph): [tradition/style] -->` comment block at the top of the draft file. The author writes their own final version from it and deletes the block when done. If no draft exists yet, note the chosen epigraph and tell the user to add it when the draft is created.
- **Alternatives** — write three more candidates with a different angle, image, or entry point into the tradition, then ask again.
- **Stop** — end without saving.

---

## Wrap-Up

If an epigraph was placed in the draft:

1. Commit with the message: `epigraph: [chapter/section name] — [date]`
2. Push to the current branch.
3. Check `CLAUDE_CODE_ENTRYPOINT`: if `cli`, skip the pull request. If not `cli`, create a pull request into `main` using `mcp__github__create_pull_request`. PR title: `epigraph: [chapter/section name] — [date]`. PR body: the chosen epigraph and the tradition or style it comes from.

---

## Tone

Epigraphs are not summaries. They are fragments from the world that carry weight before the chapter begins and different weight after. Prefer compression over completeness. Prefer strangeness over easy meaning. The reader should feel something before they understand why.

## Style Rules

Never use em dashes (---, --, or the character "—") in any output or candidate epigraph. Use a period, comma, or rephrase instead.
