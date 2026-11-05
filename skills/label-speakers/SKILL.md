---
name: label-speakers
description: Put names on the anonymous speaker groups in a diarized transcript, such as SPEAKER_00 and SPEAKER_01. Use before summarizing an episode whose transcript has speaker groups but no names.
---

# Label speakers

A diarized transcript groups cues that sound like one voice, under ids such as `SPEAKER_00`. Each group is one voice. The same name on two groups means one person split in two. You decide what each group is. Any timed transcript with speaker groups works. Optional: `uvx podcue <audio>` (https://github.com/podbox-sh/podcue) produces timed, speaker-grouped cues.

If the transcript already names its speakers, as a show's own transcript often does, check a few cues per name and skip the rest of this skill.

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

Add up each group's spoken time: the sum of its cue durations. Label the largest groups first. Stop when about 95 percent of spoken time has a label other than `undetermined`. Leave the small leftover groups `undetermined`. That does not block a summary.

## Output

Write the labels next to the transcript as `speakers.yaml`, or apply them to the transcript itself if the user prefers. Give the evidence for each name so the user can check it.

```yaml
SPEAKER_00:
  label: Tom Lee
  role: host
  evidence: "t2100: 'This is City Desk, I'm Tom Lee.'"
SPEAKER_01:
  label: Ana Ruiz
  role: guest
  evidence: "t15400: Tom says 'Welcome, Ana.' SPEAKER_01 answers next."
SPEAKER_02:
  label: quote
  evidence: "t610000: Tom says 'Here's the mayor last week.'"
SPEAKER_03:
  label: undetermined
```

Tell the user the share of spoken time you labeled, and any name you are unsure of.
