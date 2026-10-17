# Brix36 integrations: Gemini CLI and Cursor

Make a film, music video, picture or motion graphics with your Brix36 account, from Gemini CLI or Cursor. You see the credit price and approve once before anything is charged. There is no subscription.

Both use the Brix36 MCP server at `https://api.brix36.com/mcp`. You sign in on the Brix36 page that opens in your browser, so no token or key is stored in this repository.

## Gemini CLI

    gemini extensions install https://github.com/socialiserapp-design/brix36-integrations

Restart Gemini CLI afterwards. The first time a Brix36 tool runs, Gemini CLI opens your browser at the Brix36 sign-in page. This needs a local browser, so it does not work over SSH without one. The extension files are `gemini-extension.json` and `GEMINI.md` at the root of this repository.

## Cursor

The Cursor plugin is in `plugins/brix36/`, listed by `.cursor-plugin/marketplace.json`. It adds the Brix36 MCP server and three skills that walk the agent through pricing, one approval and safe retries. To add the server by hand instead, put this in `.cursor/mcp.json` or `~/.cursor/mcp.json`:

    {
      "mcpServers": {
        "brix36": { "url": "https://api.brix36.com/mcp" }
      }
    }

## What you need

A Brix36 account. New work uses that account's credits. Finding, playing and downloading your finished videos costs nothing.

## Links

- Website: https://brix36.com
- Setup help: https://brix36.com/connect
- Privacy: https://legal.brix36.com/privacy
- Terms: https://legal.brix36.com/terms
- Support: https://legal.brix36.com/support
