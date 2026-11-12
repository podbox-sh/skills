# Show cover

A show cover is optional. If you can make a picture, set it with `set_cover`. If you cannot, skip it. Do not ask the person to upload one. You may follow an aesthetic the person asks for. An episode without its own picture keeps the show cover. When the show has none, the episode keeps the PodBox default. An episode's own picture is [make-episode-cover.md](make-episode-cover.md).

## Default cover

Set the show cover only while the show has the PodBox default. Check before every `set_cover`. Read the feed at the `feed_url` link. While the show has the default, the channel `<itunes:image>` href ends in `/covers/show.png`. Any other href is a cover the person or another agent already set. Keep it, even when you think you can make a better one. Replace it only when the person asks for a new show cover.

## Palette

The show cover sets the palette. Each show picks its own palette for its topic and mood. Do not reuse PodBox brand colors by default. Episode covers reuse these exact colors. Change the object on an episode cover. Do not change the palette.

## Drawing

The object stands for the show, not for one episode's topic.

Draw one everyday object. Center it so it fills about 40 to 50 percent of the frame, with empty space around it. Draw it flat in 2D. One uniform thick line with rounded ends, and one flat accent filling part of the object. The background is one solid color. No gradients, photorealism, 3D, texture, or shading. Three colors at most. Background, line, and accent.

No text, letters, numbers, logos, brand marks, real people's likenesses, or copyrighted characters. Skip brains, robots, circuit boards, glowing orbs, and lens flares. The object must still be recognizable at 60 px.

## File

JPEG or PNG, at most 1 MiB. It must be a square from 1400 to 3000 px, and the bytes must match the contentType, or the cover is rejected. Make it 1400 × 1400 px. A 3000 px image may not fit in 1 MiB, and a flat three-color 1400 px PNG fits well under it. If your image tool returns a smaller square, and many return 1024 × 1024, resize it up to 1400 × 1400 before you send it. Do not send a smaller square. The image must be RGB with no transparency, so no alpha channel and no transparent pixels. Save a PNG as plain RGB, and flatten it onto the solid background color first. Grayscale and CMYK images are out.

## Call

`set_cover` takes `contentType` (`image/jpeg` or `image/png`) and `image` (standard base64 of the file).

## Prompt

Fill in the object and the three colors.

Flat 2D minimal illustration of a single [object], drawn with one thick uniform rounded line in [line color] and one flat [accent color] fill, centered on a solid [background color] background, lots of empty space, no text, no letters, no gradients, no shading, square, 1400 × 1400 px, RGB, no transparency.
