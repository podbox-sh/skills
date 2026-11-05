# New episode to PodBox feed

Turn each new episode of a show the user follows into a short, cited episode in their PodBox feed.

1. **Find new episodes.** With [monitor-show](../skills/monitor-show/SKILL.md), poll the feeds.
2. **Get a timed transcript.** Look for a readable one first: the show's own transcript, then the video's captions, then the RSS item's `podcast:transcript` link. Download the audio and transcribe it only when none exists and you can transcribe. With no transcript and no way to transcribe, skip the episode and tell the user why; never summarize from the show notes alone. Then take the best case of the [transcript ladder](../skills/summarize-episode/SKILL.md#the-transcript): (a) it names its speakers, (b) it has anonymous voice groups, (c) it has no speaker info, (d) audio only. Optional: `uvx podcue start <wav>` (https://github.com/podbox-sh/podcue). With podcue, steps 3 and 4 are its [Agent workflow](https://github.com/podbox-sh/podcue#agent-workflow).
3. **Label the voice groups** with [label-speakers](../skills/label-speakers/SKILL.md), in case (b) only.
4. **Summarize** with [summarize-episode](../skills/summarize-episode/SKILL.md). The result is a summary file whose paragraphs cite the transcript. Store its `episodeId` in the episode's `episode_id` column.
5. **Write the script** with [write-for-listening](../skills/write-for-listening/SKILL.md), from that summary file.
6. **Publish** the script to the user's PodBox feed. Follow the PodBox setup skill at https://podbox.sh/skill/setup.md.
7. **Mark the episode `done`** in the feeds database.

Keep the transcript, `labels.json`, and the summary file next to each other, so the user can check any sentence in the episode against the source. podcue already keeps them together in the episode folder.
