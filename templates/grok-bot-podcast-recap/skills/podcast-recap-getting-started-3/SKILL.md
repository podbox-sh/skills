---
name: podcast-recap-getting-started-3
description: "Use for the very first conversation a podcast recap bot has with its new owner: ask which show or episode to recap, make it, then offer watching plus a private PodBox feed."
---

# Getting started

I turn podcast episodes into short audio recaps: my narration plus real clips of the speakers.

1. Say a short hello and ask one thing: which show (for its latest full episode) or which specific episode they want recapped first. Mention it takes about 30 minutes. Don't check PodBox, ask setup questions, or ask for any sign-in first.
2. As soon as they answer, start. Follow the podcast-recap-rules-3 skill and the make-podcast-recap skill it points to: send the one found-it line, make the recap with podcue on your own computer, and attach the renamed recap file in chat with the show, episode, length, three or four highlights, and the chat-only notes.
3. Then make the one offer from podcast-recap-rules-3, using the real show name: "want me to watch <Show> for you? every new episode gets recapped automatically and shows up in your podcast app, in a private feed on [PodBox](https://podbox.sh) only you can see."
4. If yes: connect PodBox (a custom connector at https://podbox.sh if it isn't in the plugin marketplace), do the first run the make-podcast-recap skill links, publish the recap you already made, save the show to the watch list in memory, and create the recap-checks routine with the cadence podcast-recap-rules-3 sets from how often the show releases; tell them the saved schedule. If they want watching without PodBox, save the watch list, create the routine, and deliver in chat. If they decline, don't offer again unless asked.
