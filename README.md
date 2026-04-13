# CFFBRW MCP Server

> Agents think once. CFFBRW runs forever.

CFFBRW is an AI-powered workflow compiler for Cloudflare's edge. Write automation in plain English markdown — or compile execution plans via MCP. CFFBRW runs them deterministically on Cloudflare Workers, Durable Objects, D1, KV, and R2. No AI tokens at runtime. Self-healing.

This repo provides the MCP server configuration for connecting AI agents (Claude Code, Cursor, Windsurf, ChatGPT, Copilot) to CFFBRW.

## Quick Setup

### Claude Code

Add to `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "cffbrw": {
      "type": "url",
      "url": "https://api.cffbrw.com/mcp",
      "headers": {
        "Authorization": "Bearer wfk_your_key_here",
        "Accept": "application/json, text/event-stream"
      }
    }
  }
}
```

### Cursor / Windsurf

Same `.mcp.json` format. Place in your project root or configure via the editor's MCP settings.

### ChatGPT / Copilot (OAuth)

Use the MCP server URL `https://api.cffbrw.com/mcp` — the client handles OAuth redirect.

## Get an API Key

1. Sign up at [cffbrw.com](https://cffbrw.com)
2. Go to [API Keys](https://cffbrw.com/api-keys)
3. Create a key — you'll get `wfk_...`

## What You Get

14 tools, 4 resources, 2 prompts.

### Tools

| Tool | Description |
|------|-------------|
| `deploy_workflow` | Create a new workflow with name, markdown, and compiled plan |
| `update_workflow` | Update an existing workflow's markdown and/or plan |
| `publish_workflow` | Make a workflow live for API triggers and cron |
| `unpublish_workflow` | Revert to draft |
| `list_workflows` | List all workflows in the workspace |
| `get_workflow` | Get workflow details (markdown, plan, status) |
| `validate_plan` | Validate a plan JSON — resolves connector intents + 9 semantic checks. **Free.** |
| `prevalidate_markdown` | AI screens markdown for issues before compilation |
| `run_workflow` | Execute a workflow, returns run ID |
| `get_run_status` | Get run status with step outputs |
| `list_runs` | List runs for a workflow |
| `schedule_workflow` | Set a cron schedule |
| `unschedule_workflow` | Remove the cron schedule |

### Resources

| Resource | What your model learns |
|----------|----------------------|
| Execution Plan Schema | The exact JSON format for plans |
| Step Types | How to configure each of the 11 step types |
| Available Tools | MCP tools registered in your workspace |
| Example Plans | Correct patterns from simple to complex |

### Prompts

| Prompt | Purpose |
|--------|---------|
| `introduce` | Explains CFFBRW MCP — run this first |
| `compile` | Guides the full compile → validate → deploy → run flow |

## Example: Agent Flow

```
User: "Automate a weekly lead report every Monday"

1. Agent reads MCP resources (schema, step types)
2. Agent generates execution plan JSON
3. Agent calls validate_plan → resolves connector intents + validates
4. Agent calls deploy_workflow
5. Agent calls schedule_workflow with cron "0 9 * * MON"
6. Done — runs every Monday, no AI tokens
```

## When to Use CFFBRW

**Use CFFBRW when you need:**
- Repeatable multi-step automation on Cloudflare edge
- Human approval gates that pause for days at zero cost
- Self-healing execution (AI fixes runtime failures automatically)
- Cron or webhook-triggered workflows
- Cost efficiency: compile once, run forever (~$0.00005/run)

**Don't use CFFBRW when:**
- The task needs fresh reasoning every time (use an agent loop)
- You want drag-and-drop (use Zapier or Make)
- It's a one-off that won't repeat

## Cloudflare Primitives

CFFBRW wraps Workers, Durable Objects, D1, KV, R2, and AgentWorkflow — so you don't manage them directly.

## 11 Step Types

`http_request` | `ai_call` | `transform` | `conditional` | `loop` | `wait` | `human_approval` | `code` | `mcp_call` | `connector` | `sub_workflow`

## Connector Catalog

Google Drive, Sheets, Calendar, Gmail, Slack, Discord, Notion, SendGrid, Resend, Webhook, Custom HTTP.

## Pricing

| Plan | Credits/mo | Workflows | MCP Servers |
|------|-----------|-----------|-------------|
| Free | 100 | 5 | 2 |
| Pro | 1,000 | 50 | 10 |
| Team | 5,000 | Unlimited | 25 |

Compile = 10 credits. Run per step = 1 credit. AI recovery = 5 credits. `validate_plan` = free.

## Links

- **Homepage:** [cffbrw.com](https://cffbrw.com)
- **Dashboard:** [cffbrw.com/workflows](https://cffbrw.com/workflows)
- **Docs:** [cffbrw.com/docs/agent-guide](https://cffbrw.com/docs/agent-guide)
- **API Reference:** [cffbrw.com/docs/api-reference](https://cffbrw.com/docs/api-reference)
- **Full LLM Reference:** [cffbrw.com/llms-full.txt](https://cffbrw.com/llms-full.txt)
- **API Base URL:** `https://api.cffbrw.com`
- **MCP Endpoint:** `https://api.cffbrw.com/mcp`

## License

MIT
