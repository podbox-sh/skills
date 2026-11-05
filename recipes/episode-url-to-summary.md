# One episode URL to a summary episode

The user gives one link, such as a podcast episode page or a YouTube video, and wants to hear a summary of it.

1. **Get the audio** from the link, as in [monitor-show's fetch-audio guide](../skills/monitor-show/references/fetch-audio.md). Skip the feeds database. If the page has a transcript or captions with times and speakers, you can use those and skip the audio.
2. **Get a transcript** with times and speaker groups. Optional: `uvx podcue <audio>` (https://github.com/podbox-sh/podcue) produces timed, speaker-grouped cues.
3. **Name the speakers** with [label-speakers](../skills/label-speakers/SKILL.md).
4. **Summarize** with [summarize-episode](../skills/summarize-episode/SKILL.md). If the user asked about one topic, put that in `request` and cite only the cues that answer it.
5. **Write the script** with [write-for-listening](../skills/write-for-listening/SKILL.md).
6. **Publish** to the user's PodBox feed. Follow the PodBox setup skill at https://podbox.sh/skill/setup.md.
