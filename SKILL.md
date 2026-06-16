---
name: agentdevx-skill
description: Give your local AI access to 50+ real-world APIs — weather, GitHub, payments, food delivery and more. Auto-bootstraps. No config needed.
version: 1.0.3
metadata:
  openclaw:
    requires:
      bins:
        - curl
    homepage: https://agentdevx.onrender.com
    primaryEnv: AGENTDEVX_API_KEY
    envVars:
      - name: AGENTDEVX_API_KEY
        required: false
        description: Auto-generated on first use. Leave blank — skill bootstraps itself.
      - name: AGENTDEVX_BASE_URL
        required: false
        description: "Default: https://agentdevx.onrender.com"
---

# AgentDevX Gateway

Give your local AI hands. Connect to 50+ real-world APIs instantly — no config, no keys, no setup.

Your AI auto-registers itself on first use and gets 75,000 free credits.

## What Your AI Can Do After Install

- 🍕 Order food
- 🌤️ Get live weather
- 💸 Check exchange rates
- 📦 Query GitHub repos
- 📅 Get public holidays
- 📧 Send emails
- 🔐 Store secrets securely
- 🧠 Remember things between sessions

## Install

```bash
openclaw skills install agentdevx-skill
```

That's it. Your AI is now connected to the real world.

## How It Works

On first use your AI automatically:
1. Generates an Ed25519 identity
2. Bootstraps with AgentDevX
3. Receives 75,000 free credits
4. Gets access to all 50+ tools

No human setup needed.

## MCP Config (Claude Desktop / Cursor / Windsurf)

```json
{
  "mcpServers": {
    "agentdevx": {
      "url": "https://agentdevx.onrender.com/mcp"
    }
  }
}
```

## Available Tools

| Tool | What It Does |
|------|-------------|
| open-weather | Live weather & forecasts |
| github-api | Repos, issues, PRs |
| exchange-rate | Live currency conversion |
| public-holidays | Holidays by country |
| acme-mailer | Send emails |
| jsonplaceholder | REST API testing |
| petstore-api | OpenAPI demo |

## Security

> ⚠️ **Trust Boundary:** AgentDevX is a third-party hosted gateway. API calls route through AgentDevX servers. All credentials encrypted at rest with AES-256-GCM. Full audit log of every call.

## Links

- 🌐 Gateway: https://agentdevx.onrender.com
- 📦 npm: https://www.npmjs.com/package/@agentdevx/install
- 🔧 SDK: https://github.com/mirajmahmudul/agentdevx-sdk
