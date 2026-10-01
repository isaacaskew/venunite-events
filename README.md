# VenuNite Events

Find US concerts, comedy, theater, trivia, festivals, community events, and
family activities by place, date, interests, and budget. Results include event
details and source links. Coverage varies by location.

This repository contains public client configurations for VenuNite's hosted
MCP server. Search needs no account, API key, or local server installation.
Client subscriptions and model usage are governed by the client provider.

**Streamable HTTP endpoint:** `https://mcp.venunite.com/v1/mcp/`

[Documentation](https://venunite.com/developers/mcp) ·
[Privacy](https://venunite.com/privacy) ·
[Service terms](https://venunite.com/terms) ·
[Support](mailto:isaac@venunite.com)

## Gemini CLI

```sh
gemini extensions install https://github.com/isaacaskew/venunite-events
```

Restart Gemini CLI, then inspect `/mcp` to confirm discovery. This extension
enables `search_events` only, so every bundled tool works without credentials.

The manifest follows the [Gemini extension format](https://geminicli.com/docs/extensions/reference/).
It connects over HTTP and contains no executable hooks or local server code.
Installing it directly does not depend on a gallery listing.

## Claude Code

Install the plugin from VenuNite's repository marketplace:

```sh
claude plugin marketplace add isaacaskew/venunite-events
claude plugin install venunite-events@venunite
```

Or add only the hosted connector:

```sh
claude mcp add --transport http venunite-events https://mcp.venunite.com/v1/mcp/
```

See the [Claude marketplace documentation](https://code.claude.com/docs/en/plugin-marketplaces).
This repository marketplace is separate from approval in the public Claude Directory.

## Claude, Perplexity, Grok, Mistral, and other remote MCP clients

In the client's custom connector settings, add the endpoint above and choose
no authentication for event search. The client must support Streamable HTTP.
Availability depends on the client's plan and administrator settings.

- [Claude custom connectors](https://support.claude.com/en/articles/11175166-getting-started-with-custom-connectors-using-remote-mcp)
- [Perplexity remote connectors](https://www.perplexity.ai/help-center/en/articles/13915507-adding-custom-remote-connectors)
- [Grok custom MCP connectors](https://docs.x.ai/grok/connectors)
- [Mistral Vibe Work MCP connectors](https://docs.mistral.ai/vibe/work/connectors/mcp-connectors) (administrator setup)

Custom installation does not imply approval or inclusion in a provider's catalog.

## GitHub Copilot / VS Code

Merge the `mcpServers` entry from [`.mcp.json`](.mcp.json) into your project's
`.mcp.json`. For the VS Code configuration format, merge
[`configs/vscode.json`](configs/vscode.json) into `.vscode/mcp.json` instead.
Preserve your existing server entries. Start the server using VS Code's MCP
controls and review its tools before use.

[VS Code MCP documentation](https://code.visualstudio.com/docs/agent-customization/mcp-servers)

## Cursor

Merge [`configs/cursor.json`](configs/cursor.json) into `.cursor/mcp.json` in
your project or `~/.cursor/mcp.json` for your user. Preserve existing entries.

[Cursor MCP documentation](https://cursor.com/docs/mcp)

## Windsurf / Devin Desktop Cascade

Open the MCP config file from Cascade's Actions menu, then merge the server
entry in [`configs/cascade.json`](configs/cascade.json). The current desktop
documentation calls this product Devin Desktop. Enable the server and review
the tool list in its MCP controls.

[Cascade MCP documentation](https://docs.devin.ai/desktop/cascade/mcp)

## Tools and optional authentication

| Tool | What it does | Authentication |
| --- | --- | --- |
| `search_events` | Searches event facts and source links using location, date and interest filters | None, subject to fair-use limits |
| `create_ticket_handoff` | Creates an attributed, preview-safe link for a selected event | A VenuNite live API key |

Clients that discover every server tool will see `create_ticket_handoff`, but
anonymous calls to it return `authentication_required`. Search results already
include VenuNite event pages with ticket links when available.

For optional authenticated access, create your own key at
[VenuNite developer keys](https://venunite.com/developers/keys), then configure
your client's secret/header mechanism to send `Authorization: Bearer <key>`.
Never commit a real key. Gemini users can add an authenticated server in their
own settings if they need the handoff tool; the bundled extension stays keyless.

A handoff does **not** purchase tickets, reserve inventory, or guarantee
availability or final prices. The user reviews its page and explicitly selects
**Continue** before leaving for the external ticket seller. Automated clients
must leave that decision to the user.

The server receives tool arguments and connection metadata. Calls record quota
and request telemetry. An uncached named location may be sent to the public
Nominatim geocoder. The connector does not request whole conversation histories,
local files, calendars, or email access.

## Try it

- Find live music in Denver this weekend under $30. Include dates and links.
- Find free community events in Denver next weekend.
- Find family-friendly activities in Boulder tomorrow.

Use a specific city and date window. Verify current event details with the
linked venue or organizer; an empty search means no matching served events.

## Development

The server runs on VenuNite infrastructure; this repository contains only
configuration and documentation. Validate the Claude manifests with:

```sh
claude plugin validate .claude-plugin/plugin.json --strict
claude plugin validate .claude-plugin/marketplace.json --strict
```

The configuration files are MIT licensed. The hosted service and event data
remain subject to VenuNite's service terms and the original sources' rights.
