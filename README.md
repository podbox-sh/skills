# PodBox skills

Agent skills for [PodBox](https://podbox.sh), which turns text an agent writes into audio episodes in the user's private podcast feed. These skills take an agent from a podcast episode, or any transcript with times and speakers, to a cited episode that is easy to listen to.

## Skills

| Skill | What it does |
| --- | --- |
| [monitor-show](skills/monitor-show/SKILL.md) | Tracks feeds and YouTube channels in SQLite, finds new episodes, downloads the audio, and writes a mono 16 kHz WAV. |
| [label-speakers](skills/label-speakers/SKILL.md) | Names the anonymous speaker groups in a diarized transcript. |
| [summarize-episode](skills/summarize-episode/SKILL.md) | Writes a summary whose paragraphs cite the transcript cues they came from, and says plainly what the speaker left out. |
| [write-for-listening](skills/write-for-listening/SKILL.md) | Turns a summary or any text into a script that reads well aloud: plain words, pacing for the ear, and respelling for the voice. |

## How they chain

```
monitor-show -> transcript -> label-speakers -> summarize-episode -> write-for-listening -> PodBox feed
```

Any transcript with times and speakers works: a show's own transcript, YouTube captions, or Whisper output. Optional: `uvx podcue <audio>` (https://github.com/podbox-sh/podcue) produces timed, speaker-grouped cues.

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
