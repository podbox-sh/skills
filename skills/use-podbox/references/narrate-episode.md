# Speech

Use this page to call `create_speech_episode` well. A recap's narration follows the same writing rules.

## Voices

Call `list_speech_presets` for the voices you may name. Generated speech is in English, with one female voice, `american-female-heart`, and one male voice, `american-male-puck`. The text is read literally. An audio file you upload can be in any language. This call cannot.

## Parts and silences

A script is up to 8 parts, in order. A text part is `{text, voice_preset}` and holds at most 50000 characters. A silence part is `{silence_ms}` from 1 to 10000 milliseconds. You can alternate the two voices. The script contains no URLs or tables. The phoneme override in Writing for the ear is the one bracket to keep. Other markdown stays out.

It publishes when rendering finishes. Pass a stable `idempotencyKey`. Reusing it returns the same episode, even after it has published. A `replay` reply carries `episode` with the current status, so call `read_episode` rather than resubmit. While narration renders, `read_episode` includes progress. Poll until the episode is published or failed.

## Writing for the ear

Write each word for the voice by [https://raw.githubusercontent.com/podbox-sh/skills/main/skills/pronounce-for-tts/SKILL.md](https://raw.githubusercontent.com/podbox-sh/skills/main/skills/pronounce-for-tts/SKILL.md). That page covers numbers, acronyms, and a phoneme override for a name the voice would miss. This applies to every narrated episode: `create_speech_episode`, local Kokoro, and recap narration. Do not put a URL, a heading, or a table in the script.

## Credit reservation

The call reserves a credit quote for the narration. It is refused with `insufficientCredits` when spendable credits are below the quote. The final charge is the real narration and file, and it may pass the quote. The episode still publishes. A failed render, or a hold still open after one hour, fails the episode and releases the reservation.

What `account_status` returns, and what to do on a refusal, is in [https://raw.githubusercontent.com/podbox-sh/skills/main/skills/use-podbox/references/index.md](https://raw.githubusercontent.com/podbox-sh/skills/main/skills/use-podbox/references/index.md).

## Local audio

On a computer, a local voice is normally higher quality and cheaper. Install a model such as Kokoro and the dependencies it needs, generate the file, and publish it with the upload steps in [https://raw.githubusercontent.com/podbox-sh/skills/main/skills/use-podbox/references/index.md](https://raw.githubusercontent.com/podbox-sh/skills/main/skills/use-podbox/references/index.md). If the system or the person declines the install, use `create_speech_episode`.

`create_speech_episode` defaults to `american-male-puck`. The other voice is `american-female-heart`.
