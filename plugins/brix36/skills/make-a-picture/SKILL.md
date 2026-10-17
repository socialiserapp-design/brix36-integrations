---
name: make-a-picture
description: Make one still picture in the connected Brix36 account from a text prompt, with optional owned uploads. Show the price and wait for one approval before anything is charged.
---

# Make a picture

Use the connected Brix36 account. A picture uses that account's credits. There is no subscription. Show the price and wait for one explicit yes before you charge anything.

1. Write the prompt with the customer. If they want their own photo to guide it, use only pictures they uploaded to Brix36 and pass those IDs as `attachmentIds`.
2. Call `studio_image_quote` with the `prompt` (and `attachmentIds` if any). It returns the exact credits and price, plus a `draftId` and a `quoteId`. Nothing is made or charged.
3. Show that exact price in the chat. If the customer changes the prompt, call `studio_image_quote` again with the same `draftId`. That reprices the same draft, and the new `quoteId` replaces the old one.
4. Wait for one explicit yes.
5. Call `studio_image_confirm` once with the `draftId` and `quoteId` from the quote they approved. Repeating the same call returns the same operation and does not charge twice, so a retry after a timeout is safe.
6. Call `studio_image_result` with the `quoteId`. If it shows progress, call it again a little later. When it is ready, show `image.url` and `downloadUrl`. The links are private and signed, and another read refreshes them without a new charge.
7. If the balance is short, repeat the credit information the tool returns and stop. Do not open a checkout and do not confirm.

Use the customer's own material, a fictional character, or a person who has given permission.
