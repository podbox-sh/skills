---
name: monitor-show
description: Watch podcast feeds and YouTube channels for new episodes, download each new episode's audio, and convert it to a mono 16 kHz WAV for transcription. Use when the user wants to follow a show, check it for new episodes, or fetch the audio of one episode.
---

# Monitor a show

Keep a small SQLite file of the shows the user follows and the episodes you have already seen. On each check, fetch every feed, find the items you have not seen, and fetch their audio.

You need Node.js 20 or newer, `sqlite3`, and `ffmpeg`. Ask the user before you install any of them.

## Steps

1. Create the database once, with the tables in [references/feeds-db.md](references/feeds-db.md). Add a row to `feed` for each show the user names.
2. Poll each feed as in [references/feeds-db.md](references/feeds-db.md#poll). Insert each new item into `episode`. An item whose `guid` is already there is not new.
3. On the first poll of a new feed, ask the user how many recent episodes to process. Mark the rest `skipped`.
4. For each episode in state `new`, download the audio and write the WAV as in [references/fetch-audio.md](references/fetch-audio.md).
5. Hand the WAV on to transcription. Optional: `uvx podcue <audio>` (https://github.com/podbox-sh/podcue) produces timed, speaker-grouped cues. If the show publishes its own transcript, or the video has captions, you can use that instead.
6. Mark the episode `done` once its episode is in the user's PodBox feed, or whatever end the user asked for.

## Finish

Tell the user which episodes were new, the WAV path for each, and any download that failed.
