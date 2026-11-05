---
name: monitor-show
description: Watch podcast feeds and YouTube channels for new episodes, and get each new episode's transcript, or its audio as a mono 16 kHz WAV when there is no transcript to read. Use when the user wants to follow a show, check it for new episodes, or fetch the transcript or audio of one episode.
---

# Monitor a show

Keep a small SQLite file of the shows the user follows and the episodes you have already seen. On each check, fetch every feed, find the items you have not seen, and get a transcript for each.

You need `sqlite3` (or any SQLite client). To fetch audio you also need Node.js 20 or newer and `ffmpeg`. Ask the user before you install any of them.

## Steps

1. Create the database once, with the tables in [references/feeds-db.md](references/feeds-db.md). Add a row to `feed` for each show the user names.
2. Poll each feed as in [references/feeds-db.md](references/feeds-db.md#poll). Insert each new item into `episode`. An item whose `guid` is already there is not new.
3. On the first poll of a new feed, ask the user how many recent episodes to process. Mark the rest `skipped`.
4. For each episode in state `new`, look for a readable transcript, in this order:
   1. The show's own transcript, often linked from the episode page.
   2. The video's captions. For YouTube: `yt-dlp --skip-download --write-subs --write-auto-subs --sub-format vtt -o "<directory>/captions" "<page-url>"`.
   3. The RSS item's `<podcast:transcript>` link (the Podcasting 2.0 namespace). Its `type` may be `text/vtt`, `application/x-subrip`, `application/json` or `text/html`.

   A readable transcript has the spoken words and a time for each line or paragraph. Show notes and the episode description are not a transcript. Save it outside any git repository.
5. Only when no readable transcript exists and you can run a transcription tool, download the audio and write the WAV as in [references/fetch-audio.md](references/fetch-audio.md). Then transcribe it with any tool that gives timed cues. Optional: `uvx podcue start <wav>` (https://github.com/podbox-sh/podcue) does this and groups the voices.
6. If there is no transcript and you have no computer to transcribe on, skip the episode and tell the user why. Leave it `new`, so the next poll tries again; shows often post a transcript later. Never summarize from the show notes alone.
7. Mark the episode `done` once its episode is in the user's PodBox feed, or whatever end the user asked for.

## Finish

Tell the user which episodes were new, the transcript or WAV path for each, which you skipped and why, and any download that failed.
