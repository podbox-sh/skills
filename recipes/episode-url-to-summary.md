# One episode URL to a summary episode

The user gives one link, such as a podcast episode page or a YouTube video, and wants to hear a summary of it.

1. **Look for a transcript** at the link, in [monitor-show's order](../skills/monitor-show/SKILL.md#steps) (steps 4 to 6). Skip the feeds database. A timed transcript or captions come first. With none, get the audio as in its [fetch-audio guide](../skills/monitor-show/references/fetch-audio.md) and transcribe it, if you can. If you can't, use an untimed transcript. With none of these, stop and tell the user why.
2. **Know your tier.** The [quality tiers](../skills/summarize-episode/SKILL.md#the-transcript) say what each transcript gives and how to handle its speakers. Optional: `uvx podcue start <audio>` (https://github.com/podbox-sh/podcue). With podcue, steps 3 and 4 are its [Agent workflow](https://github.com/podbox-sh/podcue#agent-workflow).
3. **Label the voice groups** with [label-speakers](../skills/label-speakers/SKILL.md), in case (b) only. The labels go in `labels.json`.
4. **Summarize** with [summarize-episode](../skills/summarize-episode/SKILL.md). If the user asked about one topic, cite only the parts of the transcript that answer it.
5. **Write the script** with [write-for-listening](../skills/write-for-listening/SKILL.md).
6. **Publish** to the user's PodBox feed. Follow the PodBox setup skill at https://podbox.sh/skill/setup.md.
