---
name: shotation-screenshot-director
description: Plan, capture, compose, review, and export polished app-store and marketing screenshot sets with Shotation. Use when asked to improve app screenshots, create a screenshot brief, choose Shotation composition templates, direct capture coverage, write app-store screenshot copy, run Shotation compose/export flows, or validate screenshot quality for Apple App Store, Google Play, or marketing/social outputs.
---

# Shotation Screenshot Director

## Overview

Use Shotation as a screenshot production system, not just a capture tool. Inspect the app and available run evidence, write a brief, choose templates for the story each card must tell, fill gaps with scenarios or captures, then export and validate the set before calling it complete.

## Headless Listing Loop (default)

This is the primary path: the agent is the art director and copywriter. By default (`compose: "generate"`) Codex designs each whole card from the real capture, so cards can be expressive: tilted devices, real elements popping out of the frame, objects and scenery around the phone. Every generated card is reviewed against the real capture and the exact copy, and regenerated once if the UI or text drifted. `compose: "layout"` is the deterministic fallback, where the model paints only the backdrop.

1. Read the codebase and write `design.md` at the app root: product in one sentence, audience, App Store category, brand colors and fonts (from the theme or tokens files, not guessed), voice, and the five jobs the app does best, each with the screen that proves it.
2. Capture real screens. Web: `shotation app-store --base-url <url> --format iphone`. Native: `shotation capture` with a scenario per screen. Use `shotation.registry.json` seed hooks so screens show realistic fictional data, never empty states or a real person's data.
3. `shotation listing plan --count 5 --category <category>` writes `shotation.listing.json` with slots in the opinionated order (hero, primary feature, trust, second feature, closer; trust moves to slot 2 for finance and health) and the best distinct screens pre-assigned.
4. Fill every card from `design.md`: `headline` (3-5 words, verb-first benefit; the only text on the card, so there is no subheadline), `backdrop` (the scene brief for the card: what surrounds the phone and what pops out of it; pop-outs must be elements really visible in that screen, never content the screen does not show), optional `replace` (`[{ "what": "No camera available tile", "with": "a live camera view of a golfer on a hitting mat" }]`) for states a capture cannot show honestly, such as simulator-only hardware placeholders or a dev account's real names, since the reviewer allows only these declared substitutions; plus `theme` colors and `imagegen.style` (one sentence of shared art direction so the set looks like a series).
   Then plan the set as one piece before any card (read `references/set-techniques.md`): `design.grid` is what never changes (device size, position, angle wherever a whole phone shows), `design.background` is the system that does change (rotating palette, progression, alternation, or one continuous world, e.g. "one dawn golf course; each card is the next hole"), each card's `shot` varies the framing (`cover`, `phone`, `lift`, `closeup`, `pair`, `collage`, `frameless`; lint warns on fewer than three kinds or three in a row), and `seams` stitches 1-2 pairs of adjacent cards into one continuous image (`[[2, 3]]`), which the renderer generates wide and cuts at the exact card boundary.
   Default to 6 cards unless the user asks for more. To give the user options, render several named directions side by side (`shotation listing styles`, then `shotation listing render --style color-block,editorial,continuous-world`) and let them pick.
5. `shotation listing lint` and fix every error. Errors are store-rejection risks: login or onboarding in slot 1, prices, "best" or "#1", calls to action, emails, unattributed quotes, headlines over 7 words.
6. `shotation listing providers` shows which image providers are ready. `shotation listing render` designs cards with Codex (no key; bills the ChatGPT plan; about 1-2 minutes per card including review). With `--compose layout` it uses the first configured API key, then Codex, for backdrops only. Read `store-screenshots/listing.review.json`: a card with blocking issues or `devOverlay: true` must not ship; recapture a release-like build if the capture shows debug UI.
7. Open every PNG under `store-screenshots/` and review it at thumbnail size: the headline must read at 25% scale, and the device must show the real screen. Iterate on copy or backdrops and re-render; backdrops are cached by prompt, so copy edits never regenerate art.

## Operating Loop

1. Inspect the app, current Shotation config, and latest run before making design decisions.
2. Draft a screenshot brief with audience, target store, card count, narrative arc, required source states, missing captures, and export targets. Use `references/screenshot-brief.md` when creating or reviewing a brief.
3. Choose templates by screenshot purpose. Use `references/template-playbook.md` when selecting, sequencing, or adapting template families.
4. Capture missing app states with the safest real workflow available. Prefer app-authored `shotation.registry.json` scenario hooks for empty, loading, error, permission, keyboard, paywall, notification, and settings states that are hard to reach.
5. Compose cards in Studio or via composition JSON. Preserve source bindings, brand fields, target choices, and existing copy unless the user asked for a rewrite.
6. Write concise card copy that makes specific product proof visible. Use `references/copy-patterns.md` when rewriting headlines. One message per card: no subheadlines.
7. Export, inspect the manifest and output dimensions, then review the images for source quality, text-safe-area issues, unreadable contrast, clipped controls, and weak narrative flow.

## Commands

Start Studio for composition work:

```bash
shotation compose studio --run latest
```

Export the current composition:

```bash
shotation compose export --run latest
```

Capture a fresh app-store run from a live app:

```bash
shotation app-store --base-url http://localhost:3000 --format iphone --open
```

Use explicit formats when the brief requires iPad, Google Play, or marketing outputs:

```bash
shotation app-store --base-url http://localhost:3000 --format iphone --format ipad-13-portrait --open
shotation compose export --run latest --target apple --target google --target marketing
```

## Quality Bar

Do not accept a screenshot set only because files exist. Verify:

- Every card has a clear job in the narrative.
- Source captures are real unless clearly labeled simulated.
- Required states are represented or listed as missing scenario work.
- Headlines are short, specific, and grounded in visible UI proof.
- Exported PNG dimensions match the target manifest.
- Manifest paths exist and point to current output files.
- Text stays inside safe areas and remains readable at store thumbnail size.
- Device/source objects are not distorted.

## References

- Read `references/screenshot-brief.md` when producing a brief, evaluating a plan, or deciding what to capture.
- Read `references/template-playbook.md` when selecting templates, sequencing cards, or matching templates to source requirements.
- Read `references/copy-patterns.md` when writing or reviewing screenshot headlines.
