---
name: agentdevx-gateway
description: Call any API securely from your agent — GitHub, Stripe, SendGrid and 50+ more. One Ed25519 identity, encrypted credential vault, full audit log.
version: 1.0.1
metadata:
  openclaw:
    requires:
      env:
        - AGENTDEVX_API_KEY
      bins:
        - curl
    primaryEnv: AGENTDEVX_API_KEY
    envVars:
      - name: AGENTDEVX_API_KEY
        required: true
        description: Your AgentDevX JWT token from POST /agents/v1/bootstrap
      - name: AGENTDEVX_BASE_URL
        required: false
        description: Override the default gateway URL
    homepage: https://agentdevx.onrender.com
---

# AgentDevX Gateway

Give your agent hands. Call any real API securely with one Ed25519 identity and an encrypted credential vault.

## Live Stats
- ✅ 53 users · 62 agents · 955 proxy calls
- ✅ 99/100 on Smithery · Security audit: Pass
- ✅ 70+ installs on ClawHub

## Setup

1. Sign up at https://agentdevx.onrender.com/signup
2. Bootstrap your agent:

```bash
PUBLIC_KEY=$(openssl rand -hex 32)
curl -X POST https://agentdevx.onrender.com/agents/v1/bootstrap \
  -H "Content-Type: application/json" \
  -d "{\"name\":\"my-agent\",\"email\":\"you@example.com\",\"public_key\":\"$PUBLIC_KEY\"}"
```

3. Export your token:
```bash
export AGENTDEVX_API_KEY=<your_access_token>
```

## What Your Agent Can Do

### Call any tool
```bash
curl -X POST https://agentdevx.onrender.com/proxy/call \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $AGENTDEVX_API_KEY" \
  -d '{"tool_name":"github-api","action":"getRepo","params":{"owner":"openai","repo":"openai-python"}}'
```

### Discover tools
```bash
curl https://agentdevx.onrender.com/tools \
  -H "Authorization: Bearer $AGENTDEVX_API_KEY"
```

### Store secrets (AES-256-GCM encrypted)
```bash
curl -X POST https://agentdevx.onrender.com/credentials \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $AGENTDEVX_API_KEY" \
  -d '{"provider_id":"stripe","type":"api_key","value":"sk_live_..."}'
```

### Save agent memory
```bash
curl -X POST https://agentdevx.onrender.com/agents/me/memory \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $AGENTDEVX_API_KEY" \
  -d '{"key":"last_task","value":"sent invoice to client"}'
```

### Real-time event stream
```bash
curl -N https://agentdevx.onrender.com/stream/events \
  -H "Authorization: Bearer $AGENTDEVX_API_KEY"
```

### Connect via MCP
```json
{
  "mcpServers": {
    "agentdevx": {
      "url": "https://agentdevx.onrender.com/mcp"
    }
  }
}
```

## Available Tools (50+)
- **github-api** — repos, issues, search
- **open-weather** — current weather, forecasts
- **exchange-rate** — live currency conversion
- **public-holidays** — holidays by country/year
- **jsonplaceholder** — free REST test API
- **petstore-api** — OpenAPI 3.0 demo
- **acme-mailer** — send emails

## Pricing
- **Free:** 75,000 credits/month — no card needed
- **Starter:** $8/month — 500 calls/day
- **Pro:** $12/month — 5,000 calls/day
- **Scale:** $29/month — unlimited

## Links
- Gateway: https://agentdevx.onrender.com
- Smithery: https://smithery.ai/servers/mirajmahmudul57/agentdevx
- SDK: https://github.com/mirajmahmudul/agentdevx-sdk
- npm: https://www.npmjs.com/package/@agentdevx/install
