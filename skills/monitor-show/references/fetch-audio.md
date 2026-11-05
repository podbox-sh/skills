# Download the audio and write the WAV

Do this when the episode has no timed transcript and you can transcribe it, even if it has an untimed one ([SKILL.md](../SKILL.md#steps), steps 4 to 6).

Download the episode audio, then write a 16-bit mono 16 kHz WAV for transcription. Write both outside any git repository. Delete the download after the WAV exists.

## From a podcast feed

Download the enclosure URL with the script in this skill, `scripts/fetch-audio.mjs`, next to `SKILL.md`.

```
node <skill-dir>/scripts/fetch-audio.mjs <url> <output-path> [expected-bytes]
```

The script writes to `<output-path>.partial` and renames it when the download completes, so a failed download leaves no half file. It refuses a destination inside a git tree that contains the script, so audio does not end up in a repository.

Pass the enclosure `length` as `expected-bytes` when the feed gives one. The script then leaves an existing file of that size in place, and fails when the download has a different size. Some feeds list a wrong length, or `0`. If the script fails on a size mismatch, run it again without the length.

## From YouTube or another page

When the episode has no feed audio URL, or the script fails, use yt-dlp. Ask the user before you install it; `uvx yt-dlp` runs it without installing. The printed line is the download path.

```
yt-dlp -x --no-playlist --print after_move:filepath -o "<directory>/download.%(ext)s" "<page-url>"
```

If that does not get the audio, read [yt-dlp's usage and options](https://github.com/yt-dlp/yt-dlp#usage-and-options).

## Other audio

If neither applies, use the tools you have to fetch or record the audio, then write the WAV below.

## Write the WAV

If the download path and the WAV path are the same file, pick a new WAV path first.

```
ffmpeg -hide_banner -loglevel error -nostdin -y -i <download> -ac 1 -ar 16000 -c:a pcm_s16le -f wav <episode>.wav
```

`-ac 1` makes it mono, `-ar 16000` sets 16 kHz, and `pcm_s16le` writes 16-bit samples. Delete `<download>` only after the WAV exists. Keep the WAV for transcription.
