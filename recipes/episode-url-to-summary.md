# One episode URL to a summary episode

The user gives one link, such as a podcast episode page or a YouTube video, and wants to hear a summary of it.

1. **Look for a transcript** at the link, in [monitor-show's order](../skills/monitor-show/SKILL.md#steps) (steps 4 to 6). Skip the feeds database. Get the audio, as in its [fetch-audio guide](../skills/monitor-show/references/fetch-audio.md), only when there is no readable transcript and you can transcribe.
2. **Get a timed transcript.** Take the best case of the [transcript ladder](../skills/summarize-episode/SKILL.md#the-transcript): (a) it names its speakers, (b) it has anonymous voice groups, (c) it has no speaker info, (d) audio only. Optional: `uvx podcue start <audio>` (https://github.com/podbox-sh/podcue). With podcue, steps 3 and 4 are its [Agent workflow](https://github.com/podbox-sh/podcue#agent-workflow).
3. **Label the voice groups** with [label-speakers](../skills/label-speakers/SKILL.md), in case (b) only. The labels go in `labels.json`.
4. **Summarize** with [summarize-episode](../skills/summarize-episode/SKILL.md). If the user asked about one topic, cite only the cues that answer it.
5. **Write the script** with [write-for-listening](../skills/write-for-listening/SKILL.md).
6. **Publish** to the user's PodBox feed. Follow the PodBox setup skill at https://podbox.sh/skill/setup.md.
