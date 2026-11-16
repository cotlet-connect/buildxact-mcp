# Buildxact MCP server

Connect Buildxact to ChatGPT, Claude and any AI agent through Cotlet.dev. Work with estimates, quotes, job schedules, variations and progress claims in your own signed-in Buildxact account. Write actions ask for approval by default.

## What you can ask

> Prepare a quote from the Hill St deck estimate and show me the preview before sharing.

> Read the job schedule and list tasks due this week.

> Draft a progress claim for my review.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its Buildxact tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 buildxact_agent.py serve
```

Or with Docker:

```bash
docker build -t buildxact-mcp .
docker run -i --rm -e COTLET_API_KEY buildxact-mcp
```

Tools: `buildxact_guide`, `buildxact_status`, `buildxact_open`, `buildxact_read`, `buildxact_click`, `buildxact_type`, `buildxact_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 buildxact_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 buildxact_agent.py status    # is Buildxact ready in your Cotlet.dev account
python3 buildxact_agent.py guide     # Buildxact screens and sign-in link
python3 buildxact_agent.py read      # open Buildxact, read the page, close the browser
python3 buildxact_agent.py tools     # list Cotlet.dev tools
python3 buildxact_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Buildxact users

**Which Buildxact user should I connect?**

Sign in with the Buildxact user whose work the agent should do. The agent works in that user's signed-in Cloud Browser.

**Does preparing a quote send it to the client?**

No. Preparing a quote returns its preview. Sharing the quote is a separate action and asks for your approval by default.

**Is Cotlet affiliated with Buildxact?**

No. Cotlet.dev is independent and not affiliated with Buildxact.

## Learn more

- [Buildxact integration and hosted tools](https://cotlet.dev/integrations/buildxact-api-mcp-server)

---

More: https://cotlet.dev/integrations/buildxact-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=buildxact

Request access: https://cotlet.dev/signup?app=buildxact&utm_source=github&utm_medium=readme&utm_campaign=buildxact

## Workflows on the hosted integration

The live integration page documents these 30 operations:

- `manage_leads`
- `manage_clients`
- `import_contacts`
- `manage_estimates`
- `manage_estimate_templates`
- `import_estimate_from_excel`
- `set_estimate_markup`
- `manage_plans`
- `share_plans_takeoffs`
- `manage_price_catalogs`
- `manage_assemblies`
- `prepare_quote`
- `share_quote`
- `manage_jobs`
- `update_job_schedule`
- `share_job_schedule`
- `manage_daily_logs`
- `manage_quote_requests`
- `manage_purchase_orders`
- `create_material_order`
- `manage_work_orders`
- `manage_invoices`
- `manage_variations`
- `amend_invoice`
- `create_credit_note`
- `set_invoice_due_days`
- `message_client`
- `share_to_client_portal`
- `get_costing_reports`
- `get_wip_report`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
