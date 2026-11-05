---
name: write-for-listening
description: Turn a summary, notes, or any text into a script that reads well aloud through PodBox's text-to-speech voice. Use when writing words that will be heard rather than read, such as a PodBox episode script.
---

# Write for listening

A listener cannot reread a sentence, see a table, or check a spelling. PodBox speaks the script with a distilled Kokoro text-to-speech model, which reads common English well and guesses at names, acronyms, numbers, and symbols. Write so that the voice says the right thing and a listener follows it the first time.

The input is usually a summary file from [summarize-episode](../summarize-episode/SKILL.md), but any text works. The output is a plain-text script: spoken words only, with no markdown, no citations, and no stage directions.

## Steps

1. Draft the script from the source text. Keep every hedge and every "he gives no reason" from the source. Do not add facts, and do not state anything more surely than the source does.
2. Edit it by the Orwell rules in [references/orwell.md](references/orwell.md).
3. Shape it for the ear by [references/pacing.md](references/pacing.md): short sentences, signposts, no lists or tables, numbers rounded and written as words.
4. Respell the words the voice will get wrong, by [references/respelling.md](references/respelling.md).
5. Read the script once from top to bottom as a listener would. Fix any sentence you would have to hear twice.

Respell and round only in the script. Keep the real spellings and exact figures in the summary file and in any written text that goes with the episode.

To publish the script as an episode in the user's PodBox feed, follow the PodBox setup skill at https://podbox.sh/skill/setup.md.
