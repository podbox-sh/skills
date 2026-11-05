# New episode to PodBox feed

Turn each new episode of a show the user follows into a short, cited episode in their PodBox feed.

1. **Find new episodes.** With [monitor-show](../skills/monitor-show/SKILL.md), poll the feeds and download each new episode's audio as a mono 16 kHz WAV.
2. **Get a transcript** with times and speaker groups. Use the show's own transcript or the video's captions if they exist. Otherwise transcribe the WAV. Optional: `uvx podcue <audio>` (https://github.com/podbox-sh/podcue) produces timed, speaker-grouped cues.
3. **Name the speakers** with [label-speakers](../skills/label-speakers/SKILL.md). Skip this if the transcript already names them.
4. **Summarize** with [summarize-episode](../skills/summarize-episode/SKILL.md). The result is a summary file whose paragraphs cite the transcript.
5. **Write the script** with [write-for-listening](../skills/write-for-listening/SKILL.md), from that summary file.
6. **Publish** the script to the user's PodBox feed. Follow the PodBox setup skill at https://podbox.sh/skill/setup.md.
7. **Mark the episode `done`** in the feeds database.

Keep the transcript, the speaker labels, and the summary file next to each other, so the user can check any sentence in the episode against the source.
