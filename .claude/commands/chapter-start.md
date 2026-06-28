# Chapter Draft Writer

Write a first draft of the chapter or section passed as `$ARGUMENTS`. If no target is given, ask the user which chapter to work on.

This skill writes a complete, connected first draft in one pass. For surfacing gaps in an existing draft beat by beat, use `/writing-block` instead.

## Instructions

1. Read `WRITING.md` in the project root. Use it to identify: the draft directory, file naming conventions, all reference documents, the chapter plan path, and the beat plan path. If `WRITING.md` does not exist, ask the user for this information before proceeding.
2. Run `git status --short` to check for unstaged changes. If the working tree is clean, run `git pull origin main`. If there are unstaged changes, skip the pull and proceed.
3. Identify the target chapter or section from `$ARGUMENTS`.
4. If a chapter plan exists (per `WRITING.md`), read it to find the chapter's high-level plan, POV, and narrative purpose.
5. If a beat plan exists (per `WRITING.md`), read it to find the beat-by-beat plan for this chapter. If no beat plan exists for this chapter, offer to draft one before proceeding — follow the same Beat Plan Drafting process described in `/writing-block`.
6. Read all reference documents listed in `WRITING.md` in full. These are your source of truth for character voice, psychology, world rules, abilities, culture, and any other established facts.
7. Read any existing drafts immediately adjacent to this chapter (the one before and the one after, if they exist), to anchor the voice and continuity. If this is the first chapter and no other drafts exist, note that and proceed.
8. Check whether a draft already exists for this chapter. If it does, read it in full and tell the user before proceeding. Ask: **"Continue from the end of the existing draft, replace it, or stop?"**

---

## Phase 1 — Plan Confirmation

Before drafting, present your understanding of what the chapter requires:

- **POV and opening image:** who, where, and what state they are in at the chapter's start
- **Arc:** what changes between the first line and the last
- **Key beats:** the 3–5 most load-bearing moments, in order
- **Closing image:** where and how the chapter ends
- **Voice notes:** one or two observations about this POV character's register and interiority, drawn from adjacent drafts and the reference documents

Ask: **"Proceed with this plan, adjust something, or stop?"**

Incorporate any adjustments before drafting.

---

## Phase 2 — Draft

Write the chapter as a complete, connected reference draft. This draft is shown for the author to read and use as a starting point — it is never saved as final prose. When saved, each beat is wrapped in a `<!-- WRITING BLOCK: [beat label] -->` comment block so the author can work through it beat by beat and write their own version.

- Cover every planned beat in order. Do not skip beats or mark them as placeholders — write them fully so the reference is useful.
- Stay in the POV character's head throughout. Use their established register, sentence rhythm, and level of interiority as shown in adjacent drafts and in the reference documents.
- Match the length and density to the beat plan. A chapter with five tight beats should read tightly.
- If the chapter has a planned epigraph tradition noted in the chapter plan, include a placeholder: `[EPIGRAPH — run /epigraph to generate]`
- Ensure all abilities, world details, and cultural references are consistent with the reference documents.

Present the full reference draft to the user.

Ask: **"Save as reference blocks, revise a section, or discard?"**

- **Save as reference blocks** — write to the draft file (or append to the end if continuing an existing draft), wrapping each beat in a `<!-- WRITING BLOCK: [beat label] -->` ... `<!-- END WRITING BLOCK -->` comment block. The author writes their own prose from these blocks, then deletes each block when done. Commit, push, and conditionally create a PR.
- **Revise [section or beat]** — the user describes what to change. Rewrite that portion and present the updated reference draft, then ask again.
- **Discard** — end without saving anything.

---

## Wrap-Up

If the draft was saved:

1. Commit with the message: `chapter-start: first draft of [chapter name] — [date]`
2. Push to the current branch.
3. Check `CLAUDE_CODE_ENTRYPOINT`: if `cli`, skip the pull request. If not `cli`, create a pull request into `main` using `mcp__github__create_pull_request`. PR title: `chapter-start: [chapter name] first draft — [date]`. PR body: the plan summary from Phase 1 and an approximate word count.

---

## Tone

This is a first draft, not a final one. Write with full commitment — no hedging, no placeholders, no "the author might consider." Make choices. The author can change them. A bold wrong sentence is more useful than a careful vague one.

## Style Rules

Never use em dashes (---, --, or the character "—") in any output or drafted prose. Use a period, comma, or rephrase instead.

## Authorship guardrail

AI-generated prose never lands directly in the draft as final text. All generated text is placed in `<!-- WRITING BLOCK: ... -->` comment blocks. The author writes their own version from these blocks and deletes each block when done.
