---
name: write-for-listening
description: Turn a summary, notes, or any text into a script that reads well aloud through PodBox's text-to-speech voice. Use when writing words that will be heard rather than read, such as a PodBox episode script.
---

# Write for listening

A listener cannot reread a sentence, see a table, or check a spelling. PodBox speaks the script with a distilled Kokoro text-to-speech model, which reads common English well and guesses at names, acronyms, numbers, and symbols. Write so that the voice says the right thing and a listener follows it the first time.

The listening habits in [references/pacing.md](references/pacing.md) are flexible defaults, not cutoffs. Lead with the substantive point, then the detail. A sentence that only announces the contents does not count as the point. Make each reference clear, and add context only when the listener would otherwise miss what it points at. Repeat a point only when that is what lets the listener recover the thread.

The input is usually a summary file from [summarize-episode](../summarize-episode/SKILL.md): its `summary` and each paragraph's `text`. Any text works. The output is a plain-text script: spoken words only, with no markdown, no citations, and no stage directions.

## Steps

1. Draft the script from the source text. Keep every hedge and every "he gives no reason" from the source, and a quote-cited summary's note that it can't be checked against timestamps. Do not add facts, and do not state anything more surely than the source does.
2. Edit it by the Orwell rules in [references/orwell.md](references/orwell.md). Those rules are a style guide. Where a listening default keeps a word a strict cut would drop, follow the reconciliation at the end of that file.
3. Shape it for the ear by [references/pacing.md](references/pacing.md): short units, the point before the detail, clear references, lists and numbers a listener can hold, and a repeat only to recover the thread.
4. Respell the words the voice will get wrong, by [references/respelling.md](references/respelling.md).
5. Read the script once from top to bottom as a listener would. Fix any sentence you would have to hear twice by repairing that sentence. A recovery line is only for a long or complex passage the listener could not hold, which [references/pacing.md](references/pacing.md) describes.

Respell and round only in the script. Keep the real spellings and exact figures in the summary file and in any written text that goes with the episode.

To publish the script as an episode in the user's PodBox feed, follow the PodBox setup skill at https://podbox.sh/skill/setup.md.
