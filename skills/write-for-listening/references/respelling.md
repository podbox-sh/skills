# Respelling for the voice

PodBox reads the script with a distilled Kokoro text-to-speech model. It says common English words correctly. For a name, a foreign word, an acronym, a symbol, or a number, it guesses from the spelling, and the guess is often wrong. Fix this by writing the word the way an English reader would sound it out.

Respell only in the spoken script. Keep the real spelling everywhere else.

## Find the right pronunciation first

- Listen to how the person says their own name in the episode, or how the host says it. A transcript that spells the name phonetically is a clue.
- Search for the person saying their name in a talk or an interview, or for a pronunciation guide from their own site.
- If you cannot find it, use the most common pronunciation, and say so to the user.

Do not respell from the spelling alone. That is the same guess the voice would make.

## How to respell

- Break the word into syllables, and spell each one as a common English word or word part: "Siobhan" becomes "Shiv-awn".
- Join the syllables with hyphens. Do not use capitals to mark stress. The voice may read a capitalized syllable as letters.
- Do not use phonetic symbols or accents. The voice reads letters, not IPA.
- Use the same respelling every time the word appears.
- Leave well-known names alone: "Obama", "Google", "Paris".

| Written | Respelled |
| --- | --- |
| Siobhan | Shiv-awn |
| Saoirse | Seer-sha |
| Nguyen | Win |
| Joaquin | Wah-keen |
| Nietzsche | Nee-cha |
| Kubernetes | Koo-ber-net-eez |
| Xi Jinping | Shee Jin-ping |

## Acronyms

- Common acronyms the voice knows, such as FBI, CEO, NASA, and AI, stay as they are.
- For an acronym the listener may not know, say the full name the first time: "the Model Context Protocol, or M C P".
- For an acronym said as letters, write the letters with spaces: "M C P", "G P T".
- For an acronym said as a word, write the word: "SaaS" becomes "sass", "GIF" becomes "jif" or "gif", as the speaker says it.

## Numbers

Write every number as words, so the voice cannot misread it.

| Written | Spoken |
| --- | --- |
| 1984 (a year) | nineteen eighty-four |
| 2026 | twenty twenty-six |
| $4.2B | about four billion dollars |
| 37% | thirty-seven percent, or more than a third |
| 3/4 | three quarters |
| 1st, 21st | first, twenty-first |
| 10–15 | ten to fifteen |
| v2.1 | version two point one |
| 5'10" | five foot ten |
| 3:30 pm | three thirty in the afternoon |
| Oct 30 | October thirtieth |

## Units and symbols

- Write units out: "km" becomes "kilometers", "GB" becomes "gigabytes", "°C" becomes "degrees Celsius".
- Write symbols out: "&" becomes "and", "#" becomes "number" or "hashtag", "@" becomes "at", "+" becomes "plus".
- Write abbreviations out: "e.g." becomes "for example", "vs." becomes "versus", "etc." becomes "and so on".

## URLs, emails, and code

- Do not read a full URL. Say the site name: "podbox.sh" becomes "podbox dot S H".
- Say an email address only if the user needs it: "ana at example dot com".
- Do not read code aloud. Say what it does.
