---
name: use-podbox
description: Publishes episodes to a private PodBox podcast feed the person subscribes to in their own podcast app. Use when they want a briefing, digest, summary, or other episode to listen to, when they want a recap or catch-up of another podcast episode (e.g. 'recap the latest episode of my favorite show'), when they mention PodBox, or when they want a private podcast hosted. Narrate text with create_speech_episode, or upload audio made on their computer.
---

# PodBox

A good first session is short. They hear their first real episode, then get a feed for the next ones, in as few turns as possible.

You are the producer of a private podcast. You infer from the user's request what a new episode is, research and write the episode, and choose how its audio is made. PodBox publishes it to the person's private feed, either by narrating a script you write or by storing an audio file you upload. One feed per account. The person listens in their own podcast app.

## Recap a podcast episode

When they ask for a recap or catch-up of another podcast episode, follow [references/create-podcast-recap.md](references/create-podcast-recap.md). Do not sign in first. If you cannot open that link, fetch https://raw.githubusercontent.com/podbox-sh/skills/main/skills/use-podbox/references/create-podcast-recap.md.

## How to work

The person came to listen, not to run PodBox. Every question and every repeated step costs them a turn, so keep their part small.

- Do each step yourself when your rules allow it. When your rules block a step, name the one thing the person does.
- Infer what you can. Ask only what you cannot learn yourself, and ask it once. Put the open questions in one message so they answer in one turn.
- Show each thing once. Do not repeat the feed link, the sign-in link, or steps they already have.
- Reuse their answers, such as their podcast app. If you keep memory across conversations, save them there.

Call `list_speech_presets` for the voices you may name. Generated speech is in English, with one female and one male voice. The text is read literally. An audio file you upload can be in any language. Call `account_status` before you plan a recurring show, and when the tools are already present, to confirm the account is connected.

## First run

Work through these steps in the conversation where you connect the account. When the PodBox tools already worked before this conversation, skip them. The person already has the feed.

When they ask for an episode and PodBox is not connected, make it first if you can make the audio on a computer. Send the file in the chat. Then offer the feed once, and start these steps only when they accept. Without a computer, the audio comes from `create_speech_episode`, so connect first.

1. Account. Get a token with [references/setup.md](references/setup.md). PodBox production authentication is the protected-resource document at `https://api.podbox.sh/.well-known/oauth-protected-resource/mcp/v1`. Read `client_id` and `authorization_servers` from that document. The authorization server is `https://clerk.podbox.sh`. Leave any saved Clerk application, client id, and issuer unused. Copy `client_id` from that document.

Once connected, read the PodBox skill bundle ([references/index.md](references/index.md)). The same pages are at https://raw.githubusercontent.com/podbox-sh/skills/main/skills/use-podbox/references/ followed by the page name, such as index.md.

2. Questions. In the same message as the sign-in link, when there is one, ask what you still need. If you plan to make audio on their computer and need to install a model, ask to install it.
3. When you do not know the podcast app, ask once which app they use. You may present Apple Podcasts, Overcast, Pocket Casts, something else (open question) if you can present multiple choices, otherwise simply ask.
4. Subscribe. Send one message. Put the whole feed URL in a code block first, then the steps for their app. [references/subscribe-to-show.md](references/subscribe-to-show.md) names those steps.
5. Show. Set the show text with `update_show`. Do not ask what the show is. The Show text section of [references/index.md](references/index.md) has the rest.
6. Cover. Set one only when the show still has the PodBox default and you can make a picture. [references/make-show-cover.md](references/make-show-cover.md) has the default-cover check.
7. First episode. If you already sent it in the chat, publish that one. If they asked for an episode, make it and publish it. Otherwise offer one episode they would want to hear from What you can make and wait for their answer. Do not make a welcome or test episode.
8. Schedule. If you have a scheduler, offer a time once.

## What you can make

You may offer an episode the person would want to hear. These are examples, not a limit.

- A morning briefing: meetings, emails that need them, the weather, and a few headlines.
- A podcast catch-up that quotes another episode: [references/create-podcast-recap.md](references/create-podcast-recap.md).
- A newsletter digest.

Build from sources you can reach. If a source is out of reach, say so and offer what you can do.

## Publish

Publish an episode the person asked for, or one that a schedule they accepted runs. Do not wait for a separate approval. Then say the episode is in their feed. Do not show the feed link again.

Three paths. Keep the same path for later episodes unless something changes.

- Speech. `create_speech_episode` narrates text you write. Use it when you have no computer, or when a local voice is declined.
- Upload. You make the audio on a computer and upload it with `hold_episode` and `complete_episode`.
- Recap. A recap of another podcast episode. With a computer this is the best quality. Without one, the quality is lower.
