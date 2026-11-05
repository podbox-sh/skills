---
name: summarize-episode
description: Summarize a podcast episode, or any timed transcript with speaker labels, into paragraphs that each cite the cues they came from. Use when the user wants a summary of an episode, interview, or talk that can be checked against the source.
---

# Summarize an episode

The input is a transcript in which each cue has a start time, an end time, its text, and a speaker. YouTube captions, Whisper output, and a show's own transcript all work. Optional: `uvx podcue <audio>` (https://github.com/podbox-sh/podcue) produces timed, speaker-grouped cues.

The output is one YAML or JSON file in the format in [references/summary-format.md](references/summary-format.md).

## Before you write

- The speakers need names. If the transcript has only labels such as `SPEAKER_00`, name them first with the [label-speakers](../label-speakers/SKILL.md) skill.
- Know the request: what the user asked you to summarize. With no narrower request, it is the whole episode.
- Read every cue. Read a long transcript in pages. Do not skip a page.

## Steps

1. Mark the parts the episode actually contains, and the cues that hold each piece. The parts and their pieces are in [references/parts.md](references/parts.md).
2. Write the paragraphs. Each paragraph has one part type, its text, the cues it cites, its time span, and its speakers.
3. Run the coverage check in [references/parts.md](references/parts.md#check-coverage). Rewrite any paragraph that misses a piece the speaker gave.
4. Write the episode summary from the paragraph text. It must not say more, or say it more certainly, than the paragraphs do.
5. Write the file. Check that every cited cue exists in the transcript and that each `startMs` and `endMs` match the cited cues.

## Rules

- Cite only cues that answer the request. A cue you do not list is not part of the paragraph, even when it sits between two cues you list.
- When a piece the part needs is missing, say so plainly: "He gives no reason." Do not invent it, and do not leave the gap unstated.
- Keep the speaker's hedges. Never state anything more confidently than the speaker did.
- Put words in quotation marks only when they match the transcript.
- Write the text by the Orwell rules in [write-for-listening](../write-for-listening/references/orwell.md): short words, active voice, no stock phrases, no word that does no work.
- Keep exact numbers and spellings in this file. Rounding and respelling for the ear happen later, in the spoken script.
