---
name: summarize-episode
description: Summarize a podcast episode, or any transcript, into paragraphs that each cite the part of the transcript they came from. Use when the user wants a summary of an episode, interview, or talk that can be checked against the source.
---

# Summarize an episode

The input is a transcript, timed if you can get one: each cue has an id or a start time, an end time, and its text. When a transcript gives only start times, a cue ends where the next one starts. YouTube captions, a VTT file, Whisper output, and a show's own transcript all work.

The output is one summary file in the format in [references/summary-format.md](references/summary-format.md). It is the same format [podcue](https://github.com/podbox-sh/podcue) writes.

## The transcript

The summary can only be as good as its source. There are four tiers, best first:

1. **Audio, and a computer to transcribe it on.** Transcribe it with any tool that gives timed cues. Optional: `uvx podcue start <audio>` gives cues with ids and voice groups and stores the summary for you; its [Agent workflow](https://github.com/podbox-sh/podcue#agent-workflow) is the shortest path. Paragraphs cite cues with times. When podcue made the cues, `podcue check` verifies the summary.
2. **A timed transcript or captions.** Paragraphs cite cues with times.
3. **An untimed transcript only**, such as an HTML page or a `podcast:transcript` of type `text/html`. Paragraphs cite short verbatim quotes instead of cues, in the [quote-cited variant](references/summary-format.md#quote-cited-summaries). The `summary` says once that it can't be checked against timestamps. `podcue check` does not apply.
4. **No transcript and no computer.** Skip the episode and tell the user why. Never summarize from show notes.

Look for a timed transcript or captions first. With none, transcribe the audio if you have a computer, even when an untimed transcript exists. Use an untimed transcript only when you can't transcribe.

In tiers 1 and 2, handle speakers by the best case you have:

- **(a) The transcript names its speakers**, such as a show's own transcript with "ANA RUIZ:" lines. Use each name as the group id and as its label: `{"groupId": "Ana Ruiz", "label": "Ana Ruiz"}` in `labels.json`. Skip label-speakers.
- **(b) The transcript has anonymous voice groups**, such as podcue's `gc0001` or another tool's `SPEAKER_00`. Label them first with [label-speakers](../label-speakers/SKILL.md).
- **(c) The transcript has no speaker info**, such as YouTube captions or a plain transcript. Put every cue in the one group `ungrouped`. It needs no label, so there is no `labels.json`. In the text, name a speaker only when the cues make it plain, such as a self-introduction or a host handoff ("Welcome, Ana", then the answer). Otherwise write "the hosts", or say the speaker is unclear. Never guess a speaker to fill a citation.

In tier 3 there are no cues and no `labels.json`. Name a speaker only as (a) or (c) would allow.

## Before you write

- Know the request: what the user asked you to summarize. With no narrower request, it is the whole episode.
- Read every cue, or in tier 3 the whole transcript. Read a long transcript in pages (with podcue, `podcue cues <episodeId> --limit 200 --after <last cue id>`). Do not skip a page.

## Steps

1. Mark the parts the episode actually contains, and the cues that hold each piece. The parts and their pieces are in [references/parts.md](references/parts.md).
2. Write the paragraphs. Each paragraph has one part type, its text, and the cues it cites. Its time span comes from those cues.
3. Run the coverage check in [references/parts.md](references/parts.md#check-coverage). Rewrite any paragraph that misses a piece the speaker gave.
4. Write the episode summary from the paragraph text. It must not say more, or say it more certainly, than the paragraphs do.
5. Write the file. With podcue, send all paragraphs in one `podcue post-paragraph` array, then `podcue set-summary`; podcue fills the times. By hand, check that every cited cue exists, the cues are in transcript order, and each `startMs` and `endMs` match the cited cues. If the cues came from podcue, run `podcue check <episodeId> <summaryId or path>`. It must print `"state": "valid"`. In tier 3, cite quotes instead of cues, and check that each one appears word for word in the transcript.

## Rules

- Cite only cues that answer the request. A cue you do not list is not part of the paragraph, even when it sits between two cues you list.
- Every voice group you cite needs a label. A group labeled `undetermined` may be cited; `podcue check` warns but passes. Say in the text that the speaker is not known. A cited group with no label fails the check.
- When a piece the part needs is missing, say so plainly: "He gives no reason." Do not invent it, and do not leave the gap unstated.
- Keep the speaker's hedges. Never state anything more confidently than the speaker did.
- Put words in quotation marks only when they match the transcript.
- Write the text by the Orwell rules in [write-for-listening](../write-for-listening/references/orwell.md): short words, active voice, no stock phrases, no word that does no work.
- Keep exact numbers and spellings in this file. Rounding and respelling for the ear happen later, in the spoken script.
