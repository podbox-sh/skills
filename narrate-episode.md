# Speech

Use this page to call `create_speech_episode` well. A recap's narration follows the same writing rules. The recap page is [podbox://skill/create-podcast-recap.md](podbox://skill/create-podcast-recap.md).

## Voices

Call `list_speech_presets` for the voices you may name. Generated speech is in English, with one female voice, `american-female-heart`, and one male voice, `american-male-puck`. The text is read literally. An audio file you upload can be in any language. This call cannot.

## Parts and silences

A script is up to 8 parts, in order. A text part is `{text, voice_preset}` and holds at most 50000 characters. A silence part is `{silence_ms}` from 1 to 10000 milliseconds. You can alternate the two voices. The script contains no URLs, markdown, or tables.

It publishes when rendering finishes. Pass a stable `idempotencyKey`. Reusing it returns the same episode, even after it has published. A `replay` reply carries `episode` with the current status, so call `read_episode` rather than resubmit. While narration renders, `read_episode` includes progress. Poll until the episode is published or failed.

## Writing for the ear

Write each word the way people say it, so the voice says it right. The voice reads the characters it is given. This applies to every narrated episode: `create_speech_episode`, local Kokoro, and recap narration.

- Acronyms follow how people say them. A word-like acronym stays as written, such as NASA, NATO, and SCOTUS. A letter-by-letter acronym is written solid, such as "FBI", "USC", "AI", and "CSV file", not spaced letters.
- Write a name in plain spelling, such as "Calacanis". Respell it only when a test listen is wrong, as one plain word, not hyphenated stress capitals such as "Cal-uh-KAH-nis". A respelling goes only in the narration. Show notes and episode text keep the real spelling.
- In the narration, write numbers as spoken: "four hundred thousand" not "400,000", "two point four percent" not "2.4%", "one point five billion dollars" not "$1.5B", and a year as said, such as "nineteen ninety-six". Show notes and episode text keep digits. A symbol is written as said, such as "and" for &.
- Before publishing, listen to each proper noun. Leave a name that already sounds right.

Do not put a URL, markdown, a heading, or a table in the script.

## Credit reservation

The call reserves a credit quote for the narration. It is refused with `insufficientCredits` when spendable credits are below the quote. The final charge is the real narration and file, and it may pass the quote. The episode still publishes. A failed render, or a hold still open after one hour, fails the episode and releases the reservation.

What `account_status` returns, and what to do on a refusal, is in [podbox://skill](podbox://skill).

## Local audio

On a computer, a local voice is normally higher quality and cheaper. Install a model such as Kokoro and the dependencies it needs, generate the file, and publish it with the upload steps in [podbox://skill](podbox://skill). If the system or the person declines the install, use `create_speech_episode`.

A recap built with podcue uses the Kokoro id `am_michael` unless the person asks for another voice. That id is not a `voice_preset`. `create_speech_episode` defaults to `american-male-puck`. The other voice is `american-female-heart`.
