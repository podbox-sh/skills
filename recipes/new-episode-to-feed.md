# New episode to PodBox feed

Turn each new episode of a show the user follows into a short, cited episode in their PodBox feed.

1. **Find new episodes.** With [monitor-show](../skills/monitor-show/SKILL.md), poll the feeds.
2. **Get a transcript**, as in monitor-show [steps 4 to 6](../skills/monitor-show/SKILL.md#steps). A timed transcript or captions come first: the show's own transcript, then the video's captions, then the RSS item's `podcast:transcript` link. With none, download the audio and transcribe it, if you can. If you can't, use an untimed transcript. With none of these, mark the episode `skipped` and tell the user why; never summarize from the show notes. The [quality tiers](../skills/summarize-episode/SKILL.md#the-transcript) say what each transcript gives and how to handle its speakers. Optional: `uvx podcue start <wav>` (https://github.com/podbox-sh/podcue). With podcue, steps 3 and 4 are its [Agent workflow](https://github.com/podbox-sh/podcue#agent-workflow).
3. **Label the voice groups** with [label-speakers](../skills/label-speakers/SKILL.md), in case (b) only.
4. **Summarize** with [summarize-episode](../skills/summarize-episode/SKILL.md). The result is a summary file whose paragraphs cite the transcript. Store its `episodeId` in the episode's `episode_id` column.
5. **Write the script** with [write-for-listening](../skills/write-for-listening/SKILL.md), from that summary file.
6. **Publish** the script to the user's PodBox feed. Follow the PodBox setup skill at https://podbox.sh/skill/setup.md.
7. **Mark the episode `done`** in the feeds database.

Keep the transcript, `labels.json`, and the summary file next to each other, so the user can check any sentence in the episode against the source. podcue already keeps them together in the episode folder.
