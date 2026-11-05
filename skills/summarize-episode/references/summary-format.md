# Summary file format

One summary is one file. This is the same format as [podcue](https://github.com/podbox-sh/podcue)'s summary file, so one format serves both paths:

- **With podcue**, don't write the file yourself. `podcue start-summary`, `post-paragraph` and `set-summary` write it under the episode folder as `summaries/<summaryId>.yaml`, and fill the times. See podcue's [Agent workflow](https://github.com/podbox-sh/podcue#agent-workflow).
- **Without podcue**, write the file by hand as YAML (`summary.yaml`) or JSON (`summary.json`) with the same keys. Put it next to the transcript unless the user names another place.

## Example

The ids, names and times below are made up.

```yaml
version: 1
episodeId: ep_3f9a1c2b7d4e5f60
summaryId: sm_8c41d07e2a9b3f15
summary: >-
  Ana Ruiz says the city allows too few new homes near jobs, and she thinks
  that is the main reason rents rise. Tom Lee agrees about supply but says
  interest rates matter as much.
paragraphs:
  - index: 1
    part: claim
    text: >-
      Ana Ruiz says the city allows too few new homes near jobs. She thinks
      this is "probably the main reason" rents rise. She gives no figures.
    cues: [c0012, c0013, c0015]
    startMs: 41000
    endMs: 88000
  - index: 2
    part: discussion
    text: >-
      Tom Lee agrees that supply is short but says interest rates matter as
      much, because builders stop when loans cost more. Ruiz does not answer
      the point about rates.
    cues: [c0040, c0041, c0044]
    startMs: 360000
    endMs: 402300
```

## Keys

The file has exactly these keys. Any other key is an error.

- `version`: the number `1`.
- `episodeId`: the episode. With podcue, its `ep_…` id. Without podcue, a stable id you choose for the episode and keep with it.
- `summaryId`: `sm_` followed by 16 hex digits. podcue makes it. By hand, make one (`openssl rand -hex 8`) or leave the key out.
- `summary`: prose for the whole episode, written from the paragraph text after the coverage check. Not empty.
- `paragraphs`: a non-empty list, in the order you want them read. Each has exactly:
  - `index`: its 1-based position in the list.
  - `part`: optional. One of `news`, `explanation`, `advice`, `claim`, `discussion`, `story`, `items`.
  - `text`: the paragraph. Not empty.
  - `cues`: the ids of the cues it cites. They must be in transcript order, and no cue twice. Two paragraphs may cite the same cue.
  - `startMs`: the start of the earliest cited cue, in milliseconds from the start of the episode.
  - `endMs`: the end of the latest cited cue.

Episode details such as the show, title, link and date are not in this file. Keep them where you track the episode, such as the [feeds database](../../monitor-show/references/feeds-db.md).

## Cue ids

Use the ids the transcript gives its cues: podcue's are `c0001`, `c0002`, …, and a VTT file may have its own. When the cues have no ids, as with most caption files, cite each cue by its start time: `t` followed by the start in milliseconds, such as `t41000`. Every id in `cues` must point to exactly one cue.

## Labels

Speaker names are not in the summary file. They go in a `labels.json` next to the transcript, written by [label-speakers](../../label-speakers/SKILL.md), or taken from the transcript when it names its speakers ([the transcript ladder](../SKILL.md#the-transcript)):

```json
{"groups": [{"groupId": "gc0001", "label": "Ana Ruiz"}, {"groupId": "gc0003", "label": "Tom Lee"}]}
```

Every voice group a paragraph cites needs an entry there. Cues with no group (`ungrouped` in podcue) need none. A cited group labeled `undetermined` is allowed: `podcue check` warns about it but passes.
