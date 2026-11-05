# PodBox skills

Agent skills for [PodBox](https://podbox.sh), which turns text an agent writes into audio episodes in the user's private podcast feed. These skills take an agent from a podcast episode, or any transcript, to a cited episode that is easy to listen to.

## Skills

| Skill | What it does |
| --- | --- |
| [monitor-show](skills/monitor-show/SKILL.md) | Tracks feeds and YouTube channels in SQLite, finds new episodes, and gets each one's transcript, or its audio as a mono 16 kHz WAV when there is no timed transcript. |
| [label-speakers](skills/label-speakers/SKILL.md) | Labels the anonymous voice groups in a timed transcript, in a `labels.json`. |
| [summarize-episode](skills/summarize-episode/SKILL.md) | Writes a summary whose paragraphs cite the transcript cues they came from, and says plainly what the speaker left out. |
| [write-for-listening](skills/write-for-listening/SKILL.md) | Turns a summary or any text into a script that reads well aloud: plain words, pacing for the ear, and respelling for the voice. |

## How they chain

```
monitor-show -> transcript -> label-speakers (only for unnamed voices) -> summarize-episode -> write-for-listening -> PodBox feed
```

The approach is the same with any tool: get a timed transcript, name its speakers where you can, and write a summary whose paragraphs cite cues with times. Any timed transcript works: a show's own transcript, YouTube captions, a VTT file, or Whisper output. A transcript with no speaker info works too; the summary then names a speaker only where the words make it plain. An untimed transcript is a lower tier: the summary cites quotes and says it can't be checked against timestamps. The [quality tiers](skills/summarize-episode/SKILL.md#the-transcript) cover each case.

[podcue](https://github.com/podbox-sh/podcue) is optional. It produces the cues and voice groups, stores the labels and summaries, and checks them. The summary and `labels.json` formats here are podcue's, so the skills work the same with or without it.

To publish to a PodBox feed, follow the PodBox setup skill at https://podbox.sh/skill/setup.md.

## Recipes

- [New episode to PodBox feed](recipes/new-episode-to-feed.md): follow a show and publish a summary of each new episode.
- [One episode URL to a summary episode](recipes/episode-url-to-summary.md): summarize a single link.

## Install

Copy a skill folder into your agent's skills directory, such as `.agents/skills/` in a project or `~/.claude/skills/`:

```
cp -R skills/summarize-episode ~/.claude/skills/
```

The skills link to each other by relative path, so install the ones you use side by side.

## License

MIT. See [LICENSE](LICENSE).
