# One episode URL to a summary episode

The user gives one link, such as a podcast episode page or a YouTube video, and wants to hear a summary of it.

1. **Get the audio** from the link, as in [monitor-show's fetch-audio guide](../skills/monitor-show/references/fetch-audio.md). Skip the feeds database. If the page has a transcript or captions with times, you can use those and skip the audio.
2. **Get a timed transcript** with cue ids (or start times) and voice groups. Optional: `uvx podcue start <audio>` (https://github.com/podbox-sh/podcue). With podcue, steps 3 and 4 are its [Agent workflow](https://github.com/podbox-sh/podcue#agent-workflow).
3. **Label the voice groups** with [label-speakers](../skills/label-speakers/SKILL.md). The labels go in `labels.json`.
4. **Summarize** with [summarize-episode](../skills/summarize-episode/SKILL.md). If the user asked about one topic, cite only the cues that answer it.
5. **Write the script** with [write-for-listening](../skills/write-for-listening/SKILL.md).
6. **Publish** to the user's PodBox feed. Follow the PodBox setup skill at https://podbox.sh/skill/setup.md.
