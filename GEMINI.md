# Brix36 for Gemini CLI

This extension connects Gemini CLI to the Brix36 MCP server at `https://api.brix36.com/mcp`. You sign in through the Brix36 page that opens in your browser, and Gemini never sees your password. Every paid step shows the credit price first and waits for the customer's approval.

## Your finished videos

- To find videos, call `studio_projects_list` (newest first). To narrow by artist, call `studio_artists_list` with `query`, then pass the `artistId` to `studio_projects_list`.
- To play a finished video, call `studio_video_preview` with its `projectId`.
- To download one, call `studio_video_export` with its `projectId`. Say the link is temporary.
- To rename one, call `studio_project_rename` with the `projectId`, the current `expectedRevision` and the new `title`.

## Make a film or music video

Use the connected Brix36 account. Generation runs on that account's credits. There is no subscription. Show the price and wait for one explicit yes before you charge anything.

1. Ask whether this is a film or a music video if the customer has not said. Use `kind` `film` or `music`, exactly as the tool lists them.
2. Call `studio_artists_list`. Use an existing artist name when the customer picks one. For a new fictional character, pass that name with `createArtist` true. Do not create a real person. A real person uses the Brix36 app consent flow.
3. Call `studio_video_plan` once. Give it a new `operationId` (8 to 128 letters, digits, `.`, `_`, `:` or `-`) and `scope` `whole`.
   - New film: `start` with `kind` `film`, `artistName` and a `title` (a film without `songTitle` needs one), with `durationMs` when the customer gives a length, plus `film` with the finished `script` and `language` (optional `cast` and `visualWorld`).
   - New music video: `start` with `kind` `music`, `artistName` and `songTitle`. If the result says `needs_song`, ask the customer to upload the song, call `studio_song_attach`, then call `studio_video_plan` again with that `projectId` and `music` (`sections` and `visualWorld`).
   - The call saves the plan and returns the quote. It charges nothing. Repeating it with the same `operationId` and input returns the same project and quote, so a retry after a timeout is safe.
4. Show the exact price from the quote in the chat. Do not confirm yet.
5. Wait for one explicit yes.
6. Call `studio_generation_confirm` once with the `projectId`, `operationId` and `quoteToken` from that quote, and the quote fields it asks for. Do not confirm again unless the customer asks for a new change and you have shown a new quote. Repeating the same confirm reconciles the same operation and does not charge twice.
7. Call `studio_generation_wait` (or `studio_generation_status`) until it returns a video link, then `studio_video_preview` to play it. Use `studio_video_status` if you need the saved project state.
8. If the balance is short, repeat the credit information the tool returns and stop. Do not open a checkout and do not confirm.

If `studio_video_plan` is not in the tool list, use the older path in this order: `studio_video_start` first, then `studio_film_script_write` with the `projectId` it returned, then `studio_generation_quote`. The script tool needs that `projectId`, so it never comes first.

For a single picture, follow "Make a picture" below. For a lyric video, visualiser, kinetic titles, photo poster or promo cards, follow "Make motion graphics" below.

Use the customer's own material, a fictional character, or a person who has given permission.

## Make a picture

Use the connected Brix36 account. A picture uses that account's credits. There is no subscription. Show the price and wait for one explicit yes before you charge anything.

1. Write the prompt with the customer. If they want their own photo to guide it, use only pictures they uploaded to Brix36 and pass those IDs as `attachmentIds`.
2. Call `studio_image_quote` with the `prompt` (and `attachmentIds` if any). It returns the exact credits and price, plus a `draftId` and a `quoteId`. Nothing is made or charged.
3. Show that exact price in the chat. If the customer changes the prompt, call `studio_image_quote` again with the same `draftId`. That reprices the same draft, and the new `quoteId` replaces the old one.
4. Wait for one explicit yes.
5. Call `studio_image_confirm` once with the `draftId` and `quoteId` from the quote they approved. Repeating the same call returns the same operation and does not charge twice, so a retry after a timeout is safe.
6. Call `studio_image_result` with the `quoteId`. If it shows progress, call it again a little later. When it is ready, show `image.url` and `downloadUrl`. The links are private and signed, and another read refreshes them without a new charge.
7. If the balance is short, repeat the credit information the tool returns and stop. Do not open a checkout and do not confirm.

Use the customer's own material, a fictional character, or a person who has given permission.

## Make motion graphics

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
