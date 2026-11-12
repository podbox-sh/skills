# Episode cover

Set an episode cover with `set_episode_cover` when you can make a picture. If you cannot, skip it. Do not ask the person to upload one. You may follow an aesthetic the person asks for. An episode without its own picture keeps the show cover. When the show has none, the episode keeps the PodBox default. The show cover is [make-show-cover.md](make-show-cover.md).

## One picture per type

A recurring type keeps one picture, and that picture anchors the type. Send that same picture again for the next episode of that type. A morning briefing and an afternoon briefing are different types and can use different pictures. A special or one-off episode can have its own picture.

Replace an episode cover when you learn a better picture for that episode.

## Palette

Reuse the show's palette. Use those exact colors on every episode cover. Do not pick a new palette for an episode. Change the object only for a different type, or for a one-off. If the show still has the PodBox default and you are allowed to set the show cover, set that cover first and then reuse its colors.

## Drawing

For a recurring type, the object stands for that type, not for one day's topic. For a one-off, it stands for that episode's specific subject, not the general category. A fusion episode is a sun in a jar, not a generic atom.

Draw one everyday object. Center it so it fills about 40 to 50 percent of the frame, with empty space around it. Draw it flat in 2D. One uniform thick line with rounded ends, and one flat accent filling part of the object. The background is one solid color. No gradients, photorealism, 3D, texture, or shading. Three colors at most. Background, line, and accent.

No text, letters, numbers, logos, brand marks, real people's likenesses, or copyrighted characters. Skip brains, robots, circuit boards, glowing orbs, and lens flares. The object must still be recognizable at 60 px.

## File

JPEG or PNG, at most 1 MiB. It must be a square from 1400 to 3000 px, and the bytes must match the contentType, or the cover is rejected. Make it 1400 × 1400 px. A 3000 px image may not fit in 1 MiB, and a flat three-color 1400 px PNG fits well under it. If your image tool returns a smaller square, and many return 1024 × 1024, resize it up to 1400 × 1400 before you send it. Do not send a smaller square. The image must be RGB with no transparency, so no alpha channel and no transparent pixels. Save a PNG as plain RGB, and flatten it onto the solid background color first. Grayscale and CMYK images are out.

## Call

`set_episode_cover` takes `contentType` (`image/jpeg` or `image/png`), `image` (standard base64 of the file), and `episodeId`.

## Prompt

Fill in the object and the three colors.

Flat 2D minimal illustration of a single [object], drawn with one thick uniform rounded line in [line color] and one flat [accent color] fill, centered on a solid [background color] background, lots of empty space, no text, no letters, no gradients, no shading, square, 1400 × 1400 px, RGB, no transparency.
