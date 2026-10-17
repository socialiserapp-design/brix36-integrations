---
name: make-motion-graphics
description: Make a Brix36 motion graphics video, such as a lyric video, beat visualiser, kinetic titles, photo poster or promo cards, from the customer's own song and pictures. Show the price and wait for one approval before rendering.
---

# Make motion graphics

Use the connected Brix36 account. A render uses that account's credits. There is no subscription. Show the price and wait for one explicit yes before you render anything.

1. Call `studio_motion_catalogue` (optional `aspect` `16:9` or `9:16`) to show the templates, the 7 genre looks and the 24 styles, with their preview videos.
2. Pick the project with `studio_projects_list`, or start one with `studio_video_start`. Use only audio and pictures the customer owns in Brix36.
3. Call `studio_motion_create` with `projectId`, a new `requestId` (8 to 128 letters, digits, `.`, `_`, `:` or `-`), `family`, `durationSeconds` (1 to 180) and `aspect`.
   - `family` is one of `lyric_video`, `beat_visualiser`, `kinetic_text`, `photo_poster` or `promo_cards`.
   - Optional choices use the exact listed values: `templateId`, `lookId`, `styleId`, `intensity`, and `style` (`bars`, `waves` or `particles`).
   - Repeating the call with the same `requestId` keeps the same saved scene. It returns the paid render quote and may return a free first-frame preview. No render runs and nothing is charged.
4. Use `studio_motion_preview` for a free first frame if you need one. If the customer edits the scene, save it with `studio_motion_update` and get a fresh price with `studio_motion_quote`. Edits make the old quote stale.
5. Show the exact price in the chat and wait for one explicit yes.
6. Call `studio_motion_confirm` once with `projectId`, `operationId`, `quoteToken`, `revisionId`, `inputDigest` and `quotedCredits`, unchanged from the approved quote. Repeating the same call reconciles the original render and does not charge twice.
7. Call `studio_motion_wait`, then `studio_motion_status` if it is still pending. Show `video.url` and the download link from `studio_motion_result`. Call `studio_motion_use` only if the customer asks to add it to the project media.
8. If the balance is short, repeat the credit information the tool returns and stop. Do not open a checkout and do not confirm.

Use the customer's own material, a fictional character, or a person who has given permission.
