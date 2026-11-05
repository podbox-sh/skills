---
name: label-speakers
description: Put labels on the anonymous voice groups in a timed transcript, such as gc0001 or SPEAKER_00. Use before summarizing an episode whose transcript groups voices but does not name them.
---

# Label speakers

A diarized transcript groups cues that sound like one voice. The group ids are whatever the transcript uses: podcue's are `gc0001`, `gc0003`, …, and other tools use ids such as `SPEAKER_00`. Each group is one voice. The same name on two groups means one person split in two. You decide what each group is. Cues with no group (`ungrouped` in podcue) have no voice and need no label.

Any timed transcript with voice groups works. Optional: `uvx podcue start <audio>` (https://github.com/podbox-sh/podcue) produces them. With podcue, read the cues in pages with `podcue cues <episodeId> --limit 200 --after <last cue id>`, and one voice with `podcue cues <episodeId> --group-id <groupId>`. A high `nearestCosine` hints that two groups are one person.

This skill is for case (b) of [the transcript](../summarize-episode/SKILL.md#the-transcript): anonymous voice groups. A transcript that names its speakers, or has no speaker info, does not need it.

## Labels

Each group gets one of:

- a person's name, such as `Ana Ruiz`;
- `quote`: a clip being played, such as an ad, a news clip, or a voicemail;
- `ending`: a song, a sign-off, or credits after the hosts say goodbye;
- `undetermined`: you cannot tell.

## Find out who is on the show

Before you read the cues, learn who to expect.

- Read the episode title, the description, and the show notes. They often name the guest.
- Search the web for the show's hosts and for this episode's guest.
- Use what you already know about the show.

This tells you which names to look for. It does not tell you which group is which. Match names to groups only from the cues.

## Heuristics

These are suggestions, not rules. Use your judgment.

- A self-introduction followed by a name may belong to that group: "I'm Ana Ruiz", "This is City Desk, I'm Tom".
- A host greeting names the other voice: "Welcome to the show, Ana." The group that answers next is probably Ana. The group that said it is probably not.
- If a group says another person's name in two or more lines, that name is probably not this group. One mention can be a stray line.
- If the line before a group says a clip is about to play ("Here's what she said last week"), the group may be a `quote`. Read the line before it and the line after it.
- If the hosts have said goodbye and the next lines are a song or a short sign-off, those lines may be the `ending`.
- A host usually opens the episode, asks the questions, and closes it. A guest usually talks most in the middle.

Do not copy a name from the group next to it. Do not match on a repeated phrase alone. Do not guess from a handoff alone.

## When to stop

A group's spoken time is the sum of its cue durations. Label the largest groups first. Stop when about 95 percent of spoken time has a label other than `undetermined`. With podcue, `groups[].speakingMs` gives each group's time, and `label-groups` returns `labeledShare`, the same measure for the whole episode: stop at about 0.95.

Label the small leftover groups `undetermined`. A summary may cite them; `podcue check` warns but passes. A cited group with no label at all fails the check.

## Output

The labels go in `labels.json` next to the transcript:

```json
{"groups": [
  {"groupId": "gc0001", "label": "Tom Lee"},
  {"groupId": "gc0003", "label": "Ana Ruiz"},
  {"groupId": "gc0042", "label": "quote"},
  {"groupId": "gc0057", "label": "undetermined"}
]}
```

With podcue, write it with `podcue label-groups <episodeId> '[{"groupId":"gc0001","label":"Tom Lee"}, …]'`. Send only the groups you are changing; podcue merges them into the episode's `labels.json`. Without podcue, write the file yourself in this shape.

Give the user the evidence for each name, citing cues by id or start time:

- `gc0001` is Tom Lee, a host. `c0001`: "This is City Desk, I'm Tom Lee."
- `SPEAKER_01` is Ana Ruiz, the guest. `t2100`: Tom says "Welcome, Ana," and this group answers next.

Tell the user the share of spoken time you labeled, and any name you are unsure of.
