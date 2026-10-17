---
name: make-a-film-or-music-video
description: Make a film or music video in the connected Brix36 account. Show one price and wait for one approval before anything is charged.
---

# Make a film or music video

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

For a single picture, use the make-a-picture skill. For a lyric video, visualiser, kinetic titles, photo poster or promo cards, use the make-motion-graphics skill.

Use the customer's own material, a fictional character, or a person who has given permission.
