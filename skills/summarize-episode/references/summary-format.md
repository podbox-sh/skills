# Summary file format

One summary is one file. Write YAML (`summary.yaml`) or JSON (`summary.json`) with the same keys. Put it next to the transcript unless the user names another place.

## Example

The names, cue ids, and times below are made up.

```yaml
version: 1
episode:
  show: City Desk
  title: Why rents keep rising
  url: https://example.com/episodes/142
  published: 2026-10-30
transcript: transcript.json
request: Summarize the whole episode.
summary: >-
  Ana Ruiz says the city allows too few new homes near jobs, and she thinks
  that is the main reason rents rise. Tom Lee agrees about supply but says
  interest rates matter as much. She gives one renter's case: rent up by a
  third in two years.
paragraphs:
  - part: claim
    text: >-
      Ana Ruiz says the city allows too few new homes near jobs. She thinks
      this is "probably the main reason" rents rise. Her reasoning is that
      demand near jobs grows faster than the number of homes the rules allow.
      She gives no figures for either.
    cues: [c0012, c0013, c0015]
    startMs: 41000
    endMs: 88000
    speakers: [Ana Ruiz]
  - part: story
    text: >-
      A renter Ruiz knows moved near her office in 2024. Two years later her
      rent had risen by a third, and she moved out of the city. Ruiz does not
      say what the renter pays now.
    cues: [c0031, c0032, c0033]
    startMs: 302500
    endMs: 341000
    speakers: [Ana Ruiz]
  - part: discussion
    text: >-
      Tom Lee agrees that supply is short but says interest rates matter as
      much, because builders stop when loans cost more. Ruiz does not answer
      the point about rates.
    cues: [c0040, c0041, c0044]
    startMs: 360000
    endMs: 402300
    speakers: [Tom Lee, Ana Ruiz]
```

## Keys

- `version`: the number `1`.
- `episode`: what the summary is about. `show`, `title`, `url`, and `published` (a date, `YYYY-MM-DD`). Leave out a key you do not know.
- `transcript`: the path or URL of the transcript you cited.
- `request`: what the user asked you to summarize, in one sentence.
- `summary`: prose for the whole episode, written from the paragraph text after the coverage check.
- `paragraphs`: the paragraphs in the order you want them read. Each has:
  - `part`: one of `news`, `explanation`, `advice`, `claim`, `discussion`, `story`, `items`.
  - `text`: the paragraph.
  - `cues`: the ids of the cues it cites, in transcript order. Two paragraphs may cite the same cue.
  - `startMs`: the start of the earliest cited cue, in milliseconds from the start of the episode.
  - `endMs`: the end of the latest cited cue, in milliseconds.
  - `speakers`: the names of the people whose cues it cites, as labeled in the transcript or by label-speakers.

## Cue ids

Use the ids the transcript gives its cues. When the cues have no ids, as with most caption files, cite each cue by its start time: `t` followed by the start in milliseconds, such as `t41000`. Either way, every id in `cues` must point to exactly one cue in the transcript.
