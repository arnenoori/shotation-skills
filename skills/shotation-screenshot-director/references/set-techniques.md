# Set-flow techniques

How the strongest App Store sets read as one piece, from a study of 41 sets (243 cards) on
recent.design. Names in parentheses are the apps whose sets show the technique.

## 1. The camera is locked; the world changes

(Section 5 below and the directions table add the counterweight: the shot varies card to
card, from cover to close-up to collage, inside the locked system.)

In almost every strong set the phone has the same size, position, and angle on every card.
Variation comes from the background and the screen content, never from the camera.

- Locked upright grid: `perplexity/` 03-06, `tolan/`, `thru/`, `cosmos/`, `claude/` 03-06,
  `linktree/` 02-07, `granola/`, `savee/` 03-06.
- Locked tilt is rare and then identical on every card: `methods/` (only the last card tilts,
  as a closer).
- Failure mode: cards generated one at a time each pick their own angle, so the set reads
  as unrelated ads.

## 2. The background is a system, not a repeat

One of four systems, chosen per set:

- Rotating palette, same texture: `granola/` (paper colors), `linktree/` (eight colors, one
  type system), `ivory-for-mastodon/` (color blocks).
- Progression: `flighty/` (black to purple to teal), `perplexity/` (photo to off-white).
- Alternation: `claude/` (coral and black).
- One continuous world: `methods/` (one sky, cloud band at the same height), `thru/` (a
  different landscape per card, same horizon line), `tolan/` (one drawn line wandering
  through every card).

## 3. Stitched seams

Adjacent cards share one image across the boundary, so a swipe feels continuous:

- One photo spanning cards 1-2: `perplexity/`, `claude/`, `savee/` (collage bleeding across).
- A device or panel straddling the seam, half on each card: `ivory-for-mastodon/` 02-03 and
  04-05, `granola/` 05, `cosmos/` 02 (cards leaking off the right edge).
- A continuous line or band: `tolan/`, `methods/`.
Stitching is used on 1-2 seams per set, not everywhere.

## 4. The hero breaks the grid

Card 1 is often brand, photo, or type with no device (`claude/`, `perplexity/`, `bump/`,
`flighty/`, `ivory-for-mastodon/`, `savee/`, `md-vinyl/` lock screen). The locked grid
starts at card 2, which makes card 1 read as a cover.

## 5. Pop-outs are one element, same scale, every card

`methods/` (one floating payout card per screen), `tolan/` (speech bubbles), `bump/`
(stickers), `flighty/` (one floating panel). The pop-out is part of the grid: same size
range and same depth on each card.

## 6. Type is fixed

Headline position, size, weight, and case never move across the set (every set above).
Emphasis is done inside the headline (`bump/` two-color split, `perplexity/` italic word).

## What this means for generation

Cards generated one at a time cannot hold a locked camera or share a seam. The set needs a
plan first (grid, background system, seams, hero treatment), then generation that honors it:
stitched groups generated as one wide image and sliced at exact card boundaries, a grid guide
passed as a reference to every card, and a final review of the whole set side by side.

## Directions

Every strong set is one choice on each of four axes. Six combinations cover nearly the whole
corpus; `shotation listing styles` lists them and `listing render --style a,b` renders them
side by side (definitions in `src/listing/styles.mjs`).

| Direction | Device | Background system | Decoration | Type | Like |
| --- | --- | --- | --- | --- | --- |
| editorial | upright, locked, bottom cropped | paper, one dark card, photo cover | none | editorial serif or light grotesk | perplexity, cosmos, claude, savee |
| color-block | upright, locked | one saturated color per card | one UI panel lifted over the edge | heavy sans | linktree, granola, ivory |
| continuous-world | upright, locked | one panorama across the set, stitched seam | one real element floats out | bold rounded sans | methods, thru, tolan |
| expressive-tilt | tilted, same angle every card | vivid gradient, hue shifts | 3D stickers and objects | condensed display, two colors | bump, md-vinyl, flighty |
| cinematic-dark | 3D perspective | dark progression, volumetric light | hero 3D object cover | clean sans, accent word | flighty, perplexity, claude |
| overlap | large, alternating sides, straddles seams | flat block per stitched pair | second UI panel overlaps | bold sans | ivory, granola, cosmos |
