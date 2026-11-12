# PodBox skill bundle

Read this after the account is connected. The website skill is the entry page and the setup page. This bundle is everything that applies after that.

Pages:

- [subscribe-to-show.md](subscribe-to-show.md) names the steps for each podcast app.
- [narrate-episode.md](narrate-episode.md) is how to use `create_speech_episode`.
- [make-show-cover.md](make-show-cover.md) is the show cover.
- [make-episode-cover.md](make-episode-cover.md) is an episode cover.
- [create-podcast-recap.md](create-podcast-recap.md) is a recap of another podcast episode.

## Feed link

`feed_url` returns the subscribe URL. The token after `/feeds/` is how the podcast app finds the feed. A URL that stops at `/feeds` does not find the feed.

The subscribe message is one message the person follows from top to bottom, in this order:

1. The whole URL in a code block. The code block contains the URL and nothing else. Write that same URL to the clipboard when you can.
2. The numbered steps for their app.

Do not add a link or button that opens the app.

[subscribe-to-show.md](subscribe-to-show.md) names the steps for each app.

Some chats offer Copy and Copy address. Copy copies the words on the screen. Copy address copies the URL. When both appear, tell the person to copy the address. When the chat has no code block, show the whole URL as text and tell them to copy that address.

Send that message once. Send it again only when they ask for the link, when they say they cannot find the show, or after `rotate_feed`. Never post the feed URL in public.

Call `rotate_feed` only when the person says the link leaked, or when they ask for a new link. Do not offer it. The old link stops working, so they subscribe again from a new subscribe message.

## Upload

`hold_episode` takes a content type of `audio/mpeg`, `audio/mp4`, `audio/aac`, `audio/wav`, or `audio/x-m4a`, up to 1 GiB, and returns a part list. PUT each part to its own address, using that part's length, then call `complete_episode`. Those addresses stay with you. Do not show them in the chat. Pass a stable `idempotencyKey` so a retry does not open a second episode. A reply with a part list is a hold to upload. A reply with status `replay` and no parts means that key already has an episode, even a published one: do not upload, read `episode` for its status or call `read_episode`. The uploaded bytes must really be the declared type: if they are not, `complete_episode` returns `badAudio`, nothing is charged, and the hold stays open for you to upload a real file. If the file does not fit the plan, `complete_episode` returns `voided` and the file is deleted.

## Episode text

The title and the description are what the podcast app shows. `hold_episode`, `create_speech_episode`, and `update_episode` write them. Keep the description short. Add detail only when they ask for it. When you later learn a fact that changes this episode, call `update_episode`. The title is 1 through 256 Unicode code points. The description is at most 4096. `hold_episode` and `update_episode` may set `durationSeconds` from 1 through 864000. `update_episode` does not change the audio.

## Episode record

`list_episodes` and `read_episode` return the id, the status, the title, the bytes, and the duration. `read_episode` includes reason when the status is closed: deleted, expired, aged, or trimmed. Use the id on later actions. While narration renders, `read_episode` includes progress. Poll until the episode is published or failed. A published speech episode includes the credits it used. Ask before you delete an episode the person did not ask to remove. Credits are not refunded.

## Refused calls

Call `account_status` when a publish is refused. It returns the plan, the spendable credit balance, credits reserved for narration, when credits expire, storage, the episode count, the episode limit, retention, libraryTrim, and `supportUrl`. `supportUrl` is the PodBox support page. libraryTrim is null until a lapsed library trim starts.

A refusal of `insufficientCredits`, `storageClosed`, or `episodeLimit` returns `link`. Over MCP that link is the PodBox support page. Over HTTP it is the dashboard subscription page. Give the person that link and wait. Do not retry the refused call.

When `storage.episodeLimit` is a number and `storage.usedEpisodes` has reached it, call `list_episodes` and name the oldest episode. Ask before you delete it. That includes an episode that was already in the feed. If they refuse, send the dashboard link. When `retention.canChange` is true, you may call `set_retention`.

Do not mention a price before a refusal that forces an upgrade. When that refusal happens, read Pricing on the PodBox site at `/pricing` and the subscription page on the dashboard. If you do not have those pages, send the dashboard link.

## Show text

Infer the title, the description, and the author. Do not ask what the show is. When the person names one of those fields, use their words. Keep the description to one short line unless they ask for more. Call `update_show` with all three when you first know them, and again when a new fact changes the show. The title and the author are at most 200 characters. The description is at most 2000.

## Scheduler

If you have a scheduler, offer one time that fits their day. A scheduled run publishes without asking and skips publishing when there is nothing new. If you have none, say so once.

## What you may offer

You may offer an episode the person would want to hear. These are examples, not a limit.

- A morning briefing: meetings, emails that need them, the weather, and a few headlines.
- A catch-up that quotes another episode is [create-podcast-recap.md](create-podcast-recap.md).
- A newsletter digest.

Build from sources you can reach. If a source is out of reach, say so and offer what you can do. The same three are in the website skill under What you can make.
