# TODO: あわねこ湯屋 Planning Follow-Up

## Missing Design Decisions

- Keep `docs/design_sheet.md` and `assets/reference/awaneko_design_sheet.png` in sync whenever the design direction changes.
- Confirm final logo treatment, including whether the steam/paw motif is part of the production logo.
- Confirm final UI font choices for Japanese and Latin text.
- Define exact palette tokens from the design sheet swatches.
- Decide screen aspect targets for LIFF mobile use.

## Missing Asset Decisions

- Define exact sprite canvas sizes and anchor points for all cat layers.
- Decide how many initial cat body, pattern, eye, mouth, tail, dirt, and accessory variants are needed for the first playable prototype.
- Confirm whether rare/fantasy cats need glow layers as separate effects or baked into body art.
- Decide whether room furniture uses isometric perspective, front-facing perspective, or mixed storybook perspective.
- Confirm which icons are required for the first UI pass.

## Technical Risks

- Future React/Vite setup must stay compatible with LIFF browser constraints.
- Asset ids should remain stable if localStorage data later migrates to Firebase.
- Generated PNG dimensions may become too large for mobile if watercolor backgrounds are exported without compression planning.
- A future manifest loader needs validation so missing assets fail clearly.

## Sprite Alignment Risks

- Body, tail, pattern, dirt, eyes, and mouth layers may drift without shared canvases.
- Accessories need per-body attachment points.
- Dirt overlays need versions that fit multiple body silhouettes.
- Reveal fog and bubbles must not hide the cat face after cleaning.

## Safe-Area Risks

- Bottom navigation can collide with iOS home indicators inside LIFF.
- Top currency counters and close buttons can collide with notches or browser chrome.
- Storybook panel borders may feel cramped on narrow screens if not responsive.

## Mobile UI Risks

- Dense reference-sheet spacing cannot be copied directly into gameplay screens.
- Small rounded labels need minimum touch target rules.
- Beige panels on warm backgrounds may lose contrast without careful borders.
- Steam effects over text can reduce readability.

## Future Implementation Tasks

- Review and approve `PLAN.md` and `assets/asset_manifest.json`.
- Review `docs/design_sheet.md` whenever new reference art or UI examples are added.
- Produce a first approved asset batch after review.
- Prototype static screen compositions before runtime logic.
- Add React/Vite only after the planning gate is approved.
- Create a future manifest-based asset loader after implementation begins.
- Test layered sprite rendering on mobile viewport sizes.
- Add LIFF integration after the core local prototype is stable.

## Open Questions For Gameplay Phase

- How are cats discovered in the puzzle/search screen?
- What does washing input feel like: tap, drag, bubble matching, or timed cleanse?
- Are rare/fantasy cats purely cosmetic, or do they affect progression?
- What does "send off" mean mechanically and emotionally?
- How many cats should appear in the bathhouse at once?
- What progression should exist without adding pressure or battle-like goals?
