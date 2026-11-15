# WeStrive MCP server

Connect WeStrive to ChatGPT, Claude and any AI agent through Cotlet.dev. Read your client list, assign training programs, schedule check-ins and manage coaching messages in your own signed-in account.

## What you can ask

> List my active clients and their assigned programs.

> Summarize client progress before my next check-in.

> Draft a program assignment and ask me before applying it.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings > Connectors > Add custom connector, URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its WeStrive tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 westrive_agent.py serve
```

Or with Docker:

```bash
docker build -t westrive-mcp .
docker run -i --rm -e COTLET_API_KEY westrive-mcp
```

Tools: `westrive_guide`, `westrive_status`, `westrive_open`, `westrive_read`, `westrive_click`, `westrive_type`, `westrive_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 westrive_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 westrive_agent.py status    # is WeStrive ready in your Cotlet.dev account
python3 westrive_agent.py guide     # WeStrive screens and sign-in link
python3 westrive_agent.py read      # open WeStrive, read the page, close the browser
python3 westrive_agent.py tools     # list Cotlet.dev tools
python3 westrive_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from WeStrive users

**How do I connect my account?**

Request access through https://cotlet.dev/signup?app=westrive, enable WeStrive and sign in at train.westrive.com in your Cotlet Cloud Browser.

**Can I edit a purchased automated program?**

WeStrive locks an automated program after its first purchase; only the WeStrive team can unlock it.

**Is this affiliated with WeStrive?**

No. Cotlet.dev is unofficial, independent and not affiliated with WeStrive.

## Learn more

- [WeStrive integration and hosted tools](https://cotlet.dev/integrations/westrive-api-mcp-server)

---

More: https://cotlet.dev/integrations/westrive-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=westrive

Request access: https://cotlet.dev/signup?app=westrive&utm_source=github&utm_medium=readme&utm_campaign=westrive

## Workflows on the hosted integration

The live integration page documents these 15 operations:

- `list_clients`
- `invite_client`
- `review_client_application`
- `manage_client`
- `manage_programs`
- `assign_program`
- `add_exercise`
- `manage_progress`
- `schedule_client_reminders`
- `set_nutrition_goals`
- `manage_surveys`
- `send_message`
- `send_mass_message`
- `manage_invoices`
- `manage_gym_team`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
