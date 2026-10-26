---
name: process-transcripts
description: 'Process every transcript (.vtt, .docx, .txt) in transcripts/in/ into a structured summary of the piece of work discussed (project, product, feature, initiative), saved in transcripts/out/, then archive the originals. Use whenever the user mentions transcripts, meeting/workshop/call recordings or notes, Teams transcripts, or asks to process, summarise or capture what was discussed in a conversation, even if they do not name the folders or file types.'
---

# Process Transcripts

Turns each transcript of a conversation about a piece of work into a
structured summary. The summaries are general-purpose context, e.g. for
creating a backlog, writing documentation, or planning. The summary records
only what was discussed; it does not prescribe how it should be used.

**Core principle: optimise for signal density.** Capture what was discussed
and decided, not how the conversation unfolded. Cut small talk, tangents,
logistics and back-and-forth; state the option chosen (naming a rejected
alternative briefly only if useful). Never invent content that wasn't said;
put uncertainty under open questions. The summaries feed later work
(backlogs, docs, plans), so an invented detail is worse than a missing one.

## Folders

All under `transcripts/`:

- [in/](../../../transcripts/in/) — transcripts to process.
- [out/](../../../transcripts/out/) — generated summaries.
- [archive/](../../../transcripts/archive/) — processed originals.

Create any that are missing. The skill takes no arguments: it processes every
file in `in/`. Contents of these folders are gitignored (only the `.gitkeep`
files are tracked), so transcripts and summaries are never committed.

## Procedure

1. **List the files** in `in/` (`.vtt`, `.docx`, `.txt`; ignore others and
   `.gitkeep` and system files such as `.DS_Store`). If there are none, tell the user and stop.

2. **For each file, separately, prepare the text.**
   - `.vtt` (preferred; Teams: Transcript → Download): read directly. Ignore
     the `WEBVTT` header, cue ids and timestamps; speaker names appear as
     `<v Name>` tags or `Name:` prefixes.
   - `.txt`: read directly. Raw text often lacks speaker names and
     punctuation; that is expected.
   - `.docx`: extract with
     ```bash
     unzip -p input.docx word/document.xml | sed -e 's#</w:p>#\n#g' -e 's#<w:tab/>#\t#g' -e 's#<[^>]*>##g' -e 's/&amp;/\&/g;s/&lt;/</g;s/&gt;/>/g;s/&quot;/"/g' > output.txt
     ```
     If `unzip`/`sed` aren't available (e.g. Windows), do the same with
     Python's `zipfile` + `re`, Node, or PowerShell. Never ask the user to
     install anything. Write intermediate files to a temp location, not to
     `out/`.

3. **Identify speakers.** Use names/labels present in the transcript. If the
   transcript has no names, leave the `speakers` line out of the front
   matter; don't guess names or ask the user. Attribute a requirement to a
   speaker, e.g. "(Alex)", only when it's clear who said it.

4. **Filter noise and draft the summary** using the
   [template](./resources/template.md).
   - Transcripts are machine-generated and contain mishearings. Silently
     correct a word only when context makes it unambiguous; if a phrase
     matters but its meaning is unclear, keep it and list it under open
     questions rather than guessing.
   - Distinguish decisions from ideas floated: record only what the group
     settled on as scope or requirements, and put undecided options under
     open questions.
   - A transcript that is cut off or covers several topics: summarise what is
     there and note the truncation under open questions.
   - `title` — short working name of the project/work discussed.
   - `date` — conversation date/time in ISO-8601 UTC (convert from local
     time, e.g. `2026-10-20T09:30:00Z`) if stated, otherwise the file's
     modified date; ask the user only if neither is available.
   - Omit any section nothing was said about. Don't fill sections with guesses.
   - Before finalising, re-read each bullet: is it a goal, requirement,
     scope decision, constraint, or genuine open question? If not, cut it.

5. **Write the summary** to `out/<original-file-name-without-extension>-summary.md`
   so it is easy to trace back to its source (e.g. `kickoff.vtt` →
   `kickoff-summary.md`). If that file exists, add a numeric suffix rather
   than overwrite (e.g. `kickoff-summary-2.md`).

6. **Archive the original** only after the summary was written successfully:
   move the file from `in/` to `archive/`, keeping its original name. Files
   that fail to process stay in `in/`.

7. **Report** the path of each summary file so the user can open it, and
   mention any file that failed.

## Notes

- Never reproduce the raw verbatim transcript in `out/`; short quotes are fine
  only when exact wording matters.
- Process files independently so one bad file never blocks the others.
