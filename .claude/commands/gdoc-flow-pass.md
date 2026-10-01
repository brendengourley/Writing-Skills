# Google Doc Flow Pass

Read a page range of the Google Doc passed as `$ARGUMENTS` (a docs.google.com URL or bare document ID, optionally followed by a page range such as `pages 7-14`) for sentence and paragraph flow, and leave every fix in the document as a tracked suggestion. If no doc is given, ask the user for the link. If no range is given, ask which pages or chapter to cover.

This command sits beside `/gdoc-suggest`. `/gdoc-suggest` critiques story, character, and continuity. This one only asks whether the text reads smoothly from one sentence and paragraph to the next, and it fixes what it finds.

Every change goes through `SUGGEST` write mode. Never write to the author's prose directly.

## Setup

1. Resolve `$ARGUMENTS` to a `documentId`. Call `mcp__Google_Docs__read_doc`. The result is large and is saved to a file, so copy it into the scratchpad and work from the copy with a script.
2. Find the page range. Export the doc to PDF and check per-page text, or start at the chapter heading, so the range is right before reading.
3. Read every paragraph in the range in order, once, as accepted-state text (skip runs with `suggestedDeletionIds`, keep runs with `suggestedInsertionIds`). Print each paragraph with its `startIndex`.
4. Do not read the beat plan or critique files first. This pass is about how the text reads. Use project canon only to avoid contradicting it.

## What counts as a flow problem

- Comma splices and run-on sentences.
- Dialogue tags that read badly (`"..." The creature spoke.` instead of `"...," she said.`).
- A pronoun with no antecedent (`her` before the person is named, `he` right after another man).
- The same word or phrase repeated close together (`made his way` twice, `looking at him` three times, `gently` twice, consecutive sentences all starting with the character's name).
- A line that points back to something the reader has not seen for many paragraphs.
- A beat that smiles at, reacts to, or follows something that never appeared.
- Telling labels that arrive before the feeling (`Uneasily`, `To his horror`).
- Clumsy transitions between paragraphs, or a paragraph that explains what the previous one already showed.
- Contradictions inside a beat (a cool expression while her hand shakes).

Prefer the smallest edit. Replace a clause, not the paragraph, unless the author asks for an expansion. Keep the author's voice and details. Do not add new canon without saying so. If a fix is a judgment call, offer it as an option in chat instead of inserting it.

## Applying edits

One suggested edit per `update_doc` call, then a fresh `read_doc`, then verify, then the next. Indices from any earlier read are stale after every write.

1. Locate the target with a script, not by counting. Use a unique context string and an exact target substring, and compute UTF-16 offsets (`len(s.encode('utf-16-le')) // 2`). Refuse if the context is not unique or sits inside an existing suggestion. If the match fails, print the paragraph's runs: earlier author edits may have split the text across runs or changed curly and straight quotes.
2. Take `requiredRevisionId` from the revision ID in the read you just did. Never type one from memory and never reuse an old one. A 400 error about the revision means the author edited the doc. Nothing was applied: re-read and recompute.
3. Send the write with `writeControl: {"writeMode": "SUGGEST", "requiredRevisionId": ...}`. A replacement is a `deleteContentRange` followed by an `insertText` at the same start index in the same call. A pure insertion is one `insertText`. A pure deletion is one `deleteContentRange`.
4. Never delete a paragraph's trailing newline. Stop one character short of it.
5. Never batch several SUGGEST edits in one call. A SUGGEST-mode deletion stays in the index space, so a later request in the same call lands in the wrong place with no error.
6. After each write, read the doc again and print the changed paragraph with insertion and deletion tags. Confirm the edit is in the intended paragraph, the prose still reads correctly, and the change shows as a suggestion rather than plain text.
7. New paragraphs are created by inserting text that contains `\n\t`, because the doc's paragraphs begin with a tab.

`EDIT` mode is reserved for removing this command's own earlier pending suggestion when the author asks for a revised version. Never use it on the author's prose, and never delete the author's `[Note: ...]` reader notes.

## Final check

- Rebuild the accepted-state text and the original-state text from the last read, and confirm the original text is unchanged wherever no edit was made.
- Search the accepted-state text for em dashes.
- Confirm every edit appears as a suggestion.

## Report

Group what changed by scene. For each change give the before and after in short form and one line on why it helps the flow. List anything noticed but left alone, with the reason (a loose reference, missing tab indents). State plainly that everything is a tracked suggestion and reversible. Do not paste the doc link unless asked.

## Style Rules

Never use em dashes (—, --, or the character "—") in any suggested text or report. Use a period, comma, or rephrase instead.

Match the manuscript's curly quotes and apostrophes when the surrounding text uses them.

Never write a suggestion that presumes the author's intent where a judgment call exists. State the observation, let them decide.
