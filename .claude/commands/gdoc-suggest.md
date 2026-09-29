# Google Doc Suggestion Reviewer

Review the Google Doc passed as `$ARGUMENTS` (a docs.google.com URL or a bare document ID) and leave feedback directly in it using Google Docs' native Suggesting mode. If no doc is given, ask the user for the link.

This command is the live-document counterpart to `/critique` and `/critique-help`. It skips the markdown critique file entirely: instead of writing annotations to a separate file, it writes them into the document itself as real, accept-or-reject suggestions, using the `mcp__Google_Docs__read_doc` and `mcp__Google_Docs__update_doc` tools.

## Why this exists

Feedback pasted into the draft as bracketed text (`[Note: ...]`) baked permanently into the body is not a suggestion, it is clutter the author has to manually delete. Google Docs already has a mechanism for exactly this: Suggesting mode. This command uses it for everything, split into two kinds of feedback that both render as suggestions but mean different things to the author:

- **Concrete edits** — a specific wording problem with an unambiguous fix (a repeated word, a dropped word, a run-on sentence, a tonal mismatch with an obvious rewrite). These become real suggested text replacements: the flawed text shows struck through, the fix shows inserted, and the author accepts or rejects the whole change in one click.
- **Analytical notes** — a structural, sequencing, or judgment-call observation with no single correct rewrite (pacing, POV register, whether a tonal choice is deliberate). These become suggested *insertions* of a bracketed note (`[Note: ...]`) placed immediately after the relevant passage. Because they are suggestions, not baked-in text, the author dismisses them with the same reject action as any other suggestion, they never require a manual cleanup pass.

Never use `EDIT` write mode against the author's own prose. Every change this command makes to the manuscript goes through `SUGGEST` write mode. `EDIT` mode is reserved for removing this command's own stale scaffolding from a previous pass (see Phase 4).

## Instructions

### Setup

1. Read `WRITING.md` if present, for reference documents, chapter plan, and beat plan paths, the same way `/critique` does. This command does not require `WRITING.md` to run, a bare doc link is enough, but when reference documents exist, cross-reference them the same way `/critique` does.
2. Resolve `$ARGUMENTS` to a `documentId`. Accept a full `docs.google.com/document/d/<ID>/edit...` URL or a bare ID.
3. Call `mcp__Google_Docs__read_doc` with that `documentId`. This returns the document as a tree of paragraphs and text runs, each with `startIndex`/`endIndex`. Read the whole thing before proposing anything.
4. If any text runs already carry `suggestedInsertionIds` or `suggestedDeletionIds` from a prior pass, note what is still pending. Don't duplicate a suggestion that already covers the same passage.

### Phase 1 — Identify issues

Read the document as `/critique` would: story and plot, characters, style and prose, continuity against any reference documents, and beat plan alignment if one exists. For each issue found, decide which bucket it belongs to:

- **Concrete** if you can write the exact replacement text with no meaningful judgment call left for the author.
- **Analytical** if the right fix depends on authorial intent you don't have (is the disorientation deliberate, what tone should this land in, where exactly should a POV handoff occur).

When in doubt, prefer Analytical. A wrong suggested rewrite is more disruptive to reject piece-by-piece than a note is to dismiss.

### Phase 2 — Compute indices precisely, never estimate

This is the part most likely to corrupt the document if rushed. Follow it exactly.

1. For every edit, take the *exact* paragraph text from the just-completed `read_doc` result (not from an earlier read, not from memory of the prose) and locate the target substring with an actual string search (e.g. Python's `str.find`), not by eyeballing character counts. Record the precise `start` and `end` offsets this produces.
2. Never reuse indices computed before a prior write in this same session. Every `update_doc` call shifts indices for everything after the edit. After any write, the only indices you can trust are ones computed from a fresh `read_doc` taken after that write.
3. When a batch contains multiple edits, order the requests by descending `startIndex` (the edit closest to the end of the document first). Applied in that order, an edit never shifts the index of an edit still waiting to be applied later in the same batch. Applying them in ascending order is how indices drift and text gets corrupted, an insertion or deletion earlier in the document shifts every index after it, silently invalidating the positions you already computed for later edits in the same call.
4. Prefer the smallest edit that fixes the problem. Replace one word, not the sentence around it, when only one word is wrong. Smaller edits are easier to verify and less likely to accidentally swallow adjacent punctuation or the paragraph's trailing newline.
5. When a run ends with the paragraph's own trailing `\n`, make sure your delete range stops one character short of it (`endIndex - 1`), so you never delete the paragraph break itself. A deleted paragraph break merges two paragraphs into one, which is easy to miss until the whole document reads wrong.

### Phase 3 — Apply as suggestions

For a **concrete edit**, submit a `deleteContentRange` for the old text immediately followed by an `insertText` at the same `startIndex` with the replacement, both in the same `update_doc` call with `writeControl: {"writeMode": "SUGGEST"}`. This renders as a single proposed replacement in the sidebar.

For an **analytical note**, submit a single `insertText` in `SUGGEST` mode, placed right after the sentence or paragraph it concerns, formatted as:

```
 [Note: <the observation, plain and specific, one to two sentences>]
```

Lead with a space so it doesn't run into the preceding word. Never claim a specific fix in a note. If you find yourself writing "change X to Y" inside a `[Note: ...]`, it belongs in Phase 3's concrete-edit path instead, as a real suggested replacement.

Batch same-mode requests together (all `SUGGEST` requests in one `update_doc` call, ordered per Phase 2 step 3). Never mix `EDIT` and `SUGGEST` requests in the same call, `writeControl.writeMode` applies to the whole call, not per-request.

### Phase 4 — Verify

After every batch that includes a `deleteContentRange`, call `mcp__Google_Docs__read_doc` again and read the changed paragraphs in full. Confirm:

- The surrounding prose still reads correctly, no missing words, no merged sentences, no duplicated fragments.
- Each edit shows up with the expected `suggestedInsertionIds`/`suggestedDeletionIds` pairing, not as plain committed text (which would mean it landed in `EDIT` mode by mistake).

If verification turns up corrupted text, fix it immediately, before doing anything else, with a direct `EDIT`-mode delete-and-reinsert of the correct original passage. Confirm the fix with one more read before continuing. Never leave a session with prose you know is damaged.

To remove a previous pass's stale notes (an issue that's been resolved, a note that no longer applies), delete them from the base document with `EDIT` mode. Match the exact original note text with `replaceAllText` rather than hand-computed index ranges where possible, it's more forgiving of an off-by-one than a `deleteContentRange` is.

### Wrap-up

Report to the user:
- How many concrete suggested edits were made, one line each (old to new, in brief).
- How many analytical notes were added, and which passages they're attached to.
- That everything is visible and reversible in the document's Suggesting mode, nothing was written directly to the manuscript.

## Style Rules

Never use em dashes (—, --, or the character "—") in any suggested text or note. Use a period, comma, or rephrase instead.

Never write a suggestion or note that presumes the author's intent where a judgment call exists. State the observation, let them decide.
