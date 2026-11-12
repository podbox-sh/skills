# Podcast episode recap

A recap is one private-feed episode about another podcast episode. It plays clips of the speakers, with narration between them. Quote a moment when hearing them say it beats a summary. These are someone else's work, so keep the clips short, usually under 20 seconds.

Narration follows [podbox://skill/narrate-episode.md](podbox://skill/narrate-episode.md). A finished mix is published from the Upload section of [podbox://skill](podbox://skill).

## Step 0. Pick a path

With a computer, use [podcue](https://github.com/podbox-sh/podcue) 0.2.2 with the recap extra. Every command is `uvx --python 3.12 --from "podcue[recap]==0.2.2" podcue`. That path is the best quality. It keeps short clips of the original speakers. You need Python 3.12, plus `ffmpeg` and `uv` on PATH. Kokoro 0.9.4 requires Python before 3.13, so the recap extra stays on 3.12. Pin that version on every command. Each command prints one JSON line. An error line is `{"kind":"error"}`.

Without a computer, use web search and a published transcript, then publish narration only. When you deliver, say the quality is lower and there are no original clips.

Then, on either path: find the episode, get a transcript, select, plan, build, and deliver.

Select, the plan, and the show notes are the same on every path. Only finding the episode, getting the transcript, and building differ.

## Find the episode

Latest means the newest full episode. Skip a short, a clip, a trailer, and a recap cut of the same show.

With a computer, run `uvx --python 3.12 --from "podcue[recap]==0.2.2" podcue fetch <URL> --out DIR` on the episode's audio URL. It writes `DIR/audio.wav`. `start` runs on that file.

Without a computer, search the web for the show and the episode so the narration is about that episode. You do not download the audio.

## Get a transcript

Use a published transcript first. Look at the feed's own transcript, then the show's site or the episode page. A published transcript is the reading copy. It is fast, and it usually spells names right.

With a computer, always run podcue even when a published transcript exists, so each clip stays on one speaker. Use the published transcript as the reading copy for names and the fact check. Run `uvx --python 3.12 --from "podcue[recap]==0.2.2" podcue start DIR/audio.wav --language <code>`. `start` is idempotent: the same audio returns the same episode id and does not reload the models. Keep the `ep_...` id `start` returns and use it on later commands. Built-in transcription is the default: Whistle for `en`, `de`, `fr`, `es`, `it`, `nl`, and `pl`, and, as an experiment, `ja`, `ko`, and `zh`. The default language is `en`.

You may run your own speech-to-text and pass it with `uvx --python 3.12 --from "podcue[recap]==0.2.2" podcue start DIR/audio.wav --transcript FILE`. That skips the built-in model. Speaker grouping and cue building still run. FILE is `{"language": "<code>", "segments": [{"startMs", "endMs", "text", "words"?: [{startMs, endMs, text}]}]}`. `language` is required unless `--language` is passed. Any code is accepted. A mismatch is an error.

Then page the cues with `uvx --python 3.12 --from "podcue[recap]==0.2.2" podcue cues <episodeId> --limit 200`, and `--after` the last cue id, until the page is short. Each cue is `{id, startMs, endMs, text, groupId, speaker}`. `speaker` holds the group id until the groups are labeled, then the assigned label, or `uncertain` or `mixed`. The one-speaker check in `build` uses it and flags a clip that contains `uncertain` or `mixed`. Then name the groups with `label-groups`, in the next section.

Without a computer, you need a published transcript. If there is none, say so and do not invent the episode.

## Multi-speaker shows

With a computer, cues use a `groupId` until you name the voice. Read one voice with `uvx --python 3.12 --from "podcue[recap]==0.2.2" podcue cues <episodeId> --limit 200 --group-id <groupId>`. Then name the voices:

`uvx --python 3.12 --from "podcue[recap]==0.2.2" podcue label-groups <episodeId> '[{"groupId":"gc0001","label":"Ada Lovelace"}]'`

Send only the groups you are changing. A label is a person's name, `undetermined`, `quote` (a played clip), or `ending`. Stop when `labeledShare` is about 0.95, which means about 95% of speaking time has a real speaker label. `labeledShare` counts only real speaker labels, not `quote`, `ending`, or `undetermined`. `undeterminedShare` is reported separately. Map the names before you select. Take each clip from one labeled group, across consecutive cues.

## Select

Do this the same way on every path. Before you write the plan, inventory the episode. Do not treat the first minute as the default source of highlights.

List the main points. For each one, note the time span, the speaker, and a one-line claim. Include decisions, disagreements, numbers, and stories. Note the format (interview, co-hosted chat, or monologue) and who is on the episode.

Sort every main point into one tier:

- **Clip**, 5 to 7 points. These are the points a listener needs. Each gets narration plus one clip, from distinct turns, never several takes of one idea. If two sides of an argument appear, clip both or gloss one.
- **Gloss**, 2 to 4 points. One or two sentences of narration, no clip and no quote. Weave each one into the bridge next to the clip it relates to. A point with no clean clip lands here.
- **Drop.** Ads, sponsor reads, promos, intros, theme music, housekeeping, and small talk, plus anything off the core message. Never mention a dropped point.

Ideas go on tape. Facts, lists, and figures go in narration.

For each clip, pick a moment that starts at the beginning of the thought, is vivid and self-contained, and needs little setup. Drop a candidate that is flat, list-like, a yes or no, or that starts mid-sentence.

## Plan

Do this the same way on every path. Write the segments in episode order.

Quote the moment where hearing them say it beats hearing us summarize it. That is a claim in their own words, the emotion or emphasis behind it, or a distinctive phrasing. Narration would flatten that. Start where that moment starts, and stop once it lands, so no bridge has to explain it. Length follows from that. As a soft guide, roughly 10 to 20 seconds. A story can run longer and a sharp line can be 5 seconds. Usually under 20 seconds. Trim setup and repetition. These are someone else's work, so keep them short. Prefer the guest, and give every host who drives the conversation at least one clip, such as a question that sets up a story, a pushback, or a reaction that moves things along. The host who drives the conversation is the person asking most of the questions in the stretch being recapped. A co-host present for only part of the episode still counts as host for that part. Give that host a clip only when their questions are in the audio. Skip it when the host narrates and the interview questions were edited out. A recap with only the guest sounds like a monologue. Each clip is one speaker and starts and ends on a sentence boundary. Leave out the other speaker's interjections. Name the speaker correctly.

Narration introduces the episode (who is on it, the format, and the main topics; preview only the clip points), bridges into each clip, and closes. Aim for 5 to 8 minutes. If the episode is under 20 minutes, aim for 3 to 5 minutes and 4 to 5 clips. Weave each glossed point into the bridge beside the related clip, in episode order. Do not save the glosses for a list at the end, and do not open on one. Use only facts stated in the episode. Add no outside background, even a date you are sure is true. Do not repeat a clip verbatim.

Write the narration the way [podbox://skill/narrate-episode.md](podbox://skill/narrate-episode.md) describes under Writing for the ear, including how each word is written the way people say it. Do not put a URL or markdown in the narration.

### Bridges

Vary the bridges. No handoff is used more than twice. "Here is the guest." twice is the limit for that sentence. Mix three kinds:

- A handoff that names the speaker with context, such as "The host asked how it started."
- A handoff that sets up the moment, such as "Then the plan failed."
- Once the voice is established, a bridge that runs straight into the clip with no handoff.

Every handoff still makes clear who is about to speak.

If a clip refers back to something ("she", "that", "back then", a callback), the bridge before that clip sets it up.

### Fact check

Before you build, list every name, number, date, and place in the narration. Each one must be found in the transcript, or you cut it. A number may appear as digits or as words. Note every cut in the chat, not in the feed.

### script.json

Write `script.json` for `podcue build`. It holds narration segments and clips, in episode order.

```json
{
  "voice": "am_michael",
  "segments": [
    {"type": "narration", "text": "Intro.", "voice": "am_michael"},
    {"type": "clip", "start": "12:03.0", "end": "12:18.0", "speaker": "the guest"}
  ]
}
```

`voice` is a Kokoro id. It is optional and defaults to `am_michael`, unless the person asks for another voice. A narration may set its own `voice`. `start` and `end` are seconds (`"90.5"`) or `[h:]mm:ss[.s]` (`"40:43.5"`). Each clip names a `speaker` and spans consecutive cues from one labeled group. Start where the moment starts and stop once it lands. Roughly 10 to 20 seconds is a soft guide. Usually under 20 seconds. Trim setup and repetition. These are someone else's work, so keep them short.

## Build

With a computer, run `uvx --python 3.12 --from "podcue[recap]==0.2.2" podcue build <episodeId> script.json --out recap.mp3`. It cuts the clips, records the narration, matches loudness, and joins the segments. It prints one JSON line and writes `report.md` next to the audio. When `build` flags a clip that contains an uncertain or mixed-speaker cue, trim that clip to clean cues or pick a different clip. Then publish `recap.mp3` with `hold_episode` and `complete_episode`. Follow the Upload section of [podbox://skill](podbox://skill).

Without a computer, do not cut clips. Write the narration from the published transcript and publish it with `create_speech_episode`. It defaults to `american-male-puck`. Follow [podbox://skill/narrate-episode.md](podbox://skill/narrate-episode.md). When you deliver, say the quality is lower and there are no original clips.

## Show notes

Do this the same way on every path. The show notes are the episode description. Keep them to about 400 to 700 characters.

- The first line is one sentence on who and what, under 150 characters, and it still means something when an app cuts it off. Do not repeat the title or the date.
- 3 to 5 short bullets on the clipped and glossed points, as phrases.
- One footer line for the disclosure. When the episode has clips, use "AI-narrated summary with clips from the original episode." When it is narration only, use "AI-narrated summary of the original episode."

Leave process notes, per-clip attributions, dropped points, and caveats out of the show notes. Put those in the chat: whether the transcript was published or from podcue, that the timestamps refer to the source audio, any inferred speaker labels, and any claim you cut. On the path without a computer, say the quality is lower. Offer the text of the narration if the person wants it.
