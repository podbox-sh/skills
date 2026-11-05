---
name: summarize-episode
description: Summarize a podcast episode, or any timed transcript, into paragraphs that each cite the cues they came from. Use when the user wants a summary of an episode, interview, or talk that can be checked against the source.
---

# Summarize an episode

The input is a timed transcript: each cue has an id or a start time, an end time, its text, and a voice group. YouTube captions, a VTT file, Whisper output, and a show's own transcript all work. Optional: `uvx podcue start <audio>` (https://github.com/podbox-sh/podcue) produces cues with ids and voice groups, and stores the summary for you. Its [Agent workflow](https://github.com/podbox-sh/podcue#agent-workflow) is the shortest path.

The output is one summary file in the format in [references/summary-format.md](references/summary-format.md). It is the same format podcue writes.

## Before you write

- The voice groups need labels. If the transcript has only ids such as `gc0001` or `SPEAKER_00`, label them first with the [label-speakers](../label-speakers/SKILL.md) skill.
- Know the request: what the user asked you to summarize. With no narrower request, it is the whole episode.
- Read every cue. Read a long transcript in pages (with podcue, `podcue cues <episodeId> --limit 200 --after <last cue id>`). Do not skip a page.

## Steps

1. Mark the parts the episode actually contains, and the cues that hold each piece. The parts and their pieces are in [references/parts.md](references/parts.md).
2. Write the paragraphs. Each paragraph has one part type, its text, and the cues it cites. Its time span comes from those cues.
3. Run the coverage check in [references/parts.md](references/parts.md#check-coverage). Rewrite any paragraph that misses a piece the speaker gave.
4. Write the episode summary from the paragraph text. It must not say more, or say it more certainly, than the paragraphs do.
5. Write the file. With podcue, send all paragraphs in one `podcue post-paragraph` array, then `podcue set-summary`; podcue fills the times. By hand, check that every cited cue exists, the cues are in transcript order, and each `startMs` and `endMs` match the cited cues. If the cues came from podcue, run `podcue check <episodeId> <summaryId or path>`. It must print `"state": "valid"`.

## Rules

- Cite only cues that answer the request. A cue you do not list is not part of the paragraph, even when it sits between two cues you list.
- Every voice group you cite needs a label. A group labeled `undetermined` may be cited; `podcue check` warns but passes. Say in the text that the speaker is not known. A cited group with no label fails the check.
- When a piece the part needs is missing, say so plainly: "He gives no reason." Do not invent it, and do not leave the gap unstated.
- Keep the speaker's hedges. Never state anything more confidently than the speaker did.
- Put words in quotation marks only when they match the transcript.
- Write the text by the Orwell rules in [write-for-listening](../write-for-listening/references/orwell.md): short words, active voice, no stock phrases, no word that does no work.
- Keep exact numbers and spellings in this file. Rounding and respelling for the ear happen later, in the spoken script.
