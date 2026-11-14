---
name: pronounce-for-tts
description: Rewrite narration into the form a voice says, including numbers, acronyms, and a phoneme override when a name would come out wrong. Use when writing or rewriting a script for create_speech_episode, local Kokoro, or a podcue recap.
---

# Pronounce for the voice

The voice reads the characters it is given, then turns them into American English sounds. This applies to `create_speech_episode`, local Kokoro, and podcue recap narration. Show notes and episode text keep the real spelling and the digits. Only the narration changes.

## Spoken form

- Acronyms follow how people say them. A word-like acronym stays as written, such as NASA, NATO, and SCOTUS. A letter-by-letter acronym is written solid, such as "FBI", "USC", "AI", and "CSV file".
- In the narration, write numbers as spoken. "four hundred thousand" not "400,000". "two point four percent" not "2.4%". "one point five billion dollars" not "$1.5B". A year is said, such as "nineteen ninety-six". A symbol is written as said, such as "and" for & and "the Chinese Communist Party" for the abbreviation.
- podcue 0.2.7 also rewrites digits, money, percents, clock times, and years in the narration before Kokoro. Writing the spoken form still works on that path and on `create_speech_episode`.

## Names that are not English

Spot a word the American voice is likely to miss. That is a name that is not English, a brand, or a word borrowed from another language.

Before you search, read `pronunciations.json` in the recap working directory. That directory is the folder that holds the episode directories from `podcue fetch`, not a folder inside one episode. Without a computer, keep the same file with the other working files for that show. If the name is already there, use its phonemes.

When it is missing, look it up on the web only. Do not download a phoneme model or an alignment model. wav2vec2 is out because of its size.

Search for the person saying their own name. Try "<name> how to pronounce my name", an interview clip where they say it, and "<name> pronunciation IPA". A generic name-guide site is a weak source and is often wrong. One said "CHAH-math" for Chamath. The name is cha-MOTH.

Trust, in order: the person themselves, then a reliable IPA note such as Wikipedia's pronunciation, then a crowd guide. If the sources disagree, tell the user and wait. Do not pick one.

Convert the pronunciation you trust into misaki phonemes, with `ˈ` immediately before the stressed vowel. Say the name once inside a short sentence and listen. Then append one object to `pronunciations.json`: `name`, `phonemes`, `source`, and `date` as `YYYY-MM-DD`. Read that file before every later narration. Do not commit it.

## The override

Two paths read the same bracket. Both keep it. Write one word inside the brackets, `[Word](/phonemes/)`. A phrase in one pair puts the sounds on the first word and drops the rest. A possessive stays outside: `[Huawei](/hwˈɑwˌA/)'s`. Put `ˈ` immediately before the stressed vowel. `ˌ` marks another syllable when it needs a weaker stress. Use only misaki's American symbols. A character outside that set is dropped, including a plain Latin `g`. The `g` sound is `ɡ`. Do not put a digit in the override.

**On a computer, podcue 0.2.7.** `say_numbers` leaves the brackets alone. Kokoro 0.9.4 then runs misaki 0.9.4 English G2P, which replaces the word with the phoneme string. Use the override.

**Hosted speech, `create_speech_episode`.** The server trims the text and does not strip brackets or parentheses. `apps/kokoro/synthesize.py` runs that same G2P. Use the override. Do not respell the name instead.

Leave the real spelling when a test listen already sounds right. Do not invent a respelling such as stress capitals. The override is the one bracket the voice reads. It is not markdown to strip. Other markdown stays out of the script.

| Word | Write this | Where the stress is | Source |
| --- | --- | --- | --- |
| Huang | [Huang](/hwˈɑŋ/) | the only syllable | last resort. The onset matches misaki's Huangshi (`hwˌɑŋʃˈi`). Not the person, and not a Wikipedia note. |
| Huawei | [Huawei](/hwˈɑwˌA/) | the first syllable, WAH-way | last resort. `A` is the vowel misaki uses for "way" (`wˈA`). Not the person, and not a Wikipedia note. |
| Chamath | pending. [Chamath](/ʧəmˈɑθ/) or [Chamath](/ʧəmˈɔθ/) | the second syllable, cha-MOTH | the stress is stated. The vowel is not chosen yet. Do not narrate it until someone listens and picks `ɑ` or `ɔ`. A crowd guide's "CHAH-math" is the weak source. |

`ˈ` sits immediately before the stressed vowel, which is how misaki's lexicon writes it (`mˈæθ`, `ʧˈInə`). An override is copied through as written. `[Chamath](/ʧəˈmɑθ/)` and `[Chamath](/ʧəmˈɑθ/)` are different token strings, `ʧəˈmɑθ` and `ʧəmˈɑθ`. The first leaves the stress mark before `m`. Use the second shape. `θ` is the th in "math". If the episode or the person says a name differently, use that and save it.
