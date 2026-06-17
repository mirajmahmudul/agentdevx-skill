---
name: agentdevx-skill
description: Call any API securely from your agent — GitHub, weather, exchange rates, email and 50+ more. Agent self-registers on first use via Ed25519 identity. Encrypted credential vault, full audit log.
version: 1.0.5
metadata:
  openclaw:
    requires:
      bins:
        - curl
    primaryEnv: AGENTDEVX_API_KEY
    envVars:
      - name: AGENTDEVX_API_KEY
        required: false
        description: Obtained automatically on first tool call via self-bootstrap (POST /agents/v1/bootstrap). No human action needed. See Transparency section.
      - name: AGENTDEVX_BASE_URL
        required: false
        description: Override the default gateway URL (default https://agentdevx.onrender.com)
    homepage: https://agentdevx.onrender.com
---

# AgentDevX Gateway

Give your agent hands. Call any real API securely — GitHub, weather, exchange rates, email — with one Ed25519 identity and an encrypted credential vault.

## Transparency — What Happens On First Use

This skill is designed to be used by an AI agent without human setup. Here is exactly what happens, so you know what to expect before your agent uses it:

1. **Identity creation:** Your agent generates an Ed25519 keypair locally. The private key never leaves your machine.
2. **Registration:** Your agent sends the public key to `https://agentdevx.onrender.com/agents/v1/bootstrap`, a third-party server not operated by ClawHub or OpenClaw. This is an outbound network call.
3. **Account creation:** AgentDevX creates a user account and agent record tied to that public key, and returns a JWT access token plus 75,000 free credits.
4. **Token storage:** The JWT is held in your agent's environment/session for subsequent calls. It is not written to disk unless your agent explicitly does so.
5. **Subsequent use:** Every tool call after this point (weather, GitHub, email, etc.) is routed through AgentDevX's proxy using that token, and is recorded in AgentDevX's audit log.

No manual signup, browser step, or credential copying is required — registration happens automatically the first time a tool from this skill is invoked. This is intentional self-provisioning, not a bug or unexpected behavior.

**If you do not want your agent to register with a third-party service, do not invoke tools from this skill.**

## Setup (Fully Automatic)

Your agent issues this on first use — no human action needed:

```bash
PUBLIC_KEY=$(openssl rand -hex 32)
BOOTSTRAP=$(curl -s -X POST https://agentdevx.onrender.com/agents/v1/bootstrap \
  -H "Content-Type: application/json" \
  -d "{\"name\":\"openclaw-agent\",\"email\":\"agent@local\",\"public_key\":\"$PUBLIC_KEY\"}")
export AGENTDEVX_API_KEY=$(echo "$BOOTSTRAP" | grep -o '"access_token":"[^"]*"' | cut -d'"' -f4)
```

## What Your Agent Can Do

### Discover available tools
```bash
curl https://agentdevx.onrender.com/tools \
  -H "Authorization: Bearer $AGENTDEVX_API_KEY"
```

### Call any tool via proxy
```bash
curl -X POST https://agentdevx.onrender.com/proxy/call \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $AGENTDEVX_API_KEY" \
  -d '{"tool_name":"github-api","action":"getRepo","params":{"owner":"openai","repo":"openai-python"}}'
```

### Store secrets securely (AES-256-GCM encrypted)
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

## Available Tools
- **github-api** — repos, issues, search
- **open-weather** — current weather, forecasts
- **exchange-rate** — live currency conversion
- **public-holidays** — holidays by country/year
- **jsonplaceholder** — free REST test API
- **petstore-api** — OpenAPI 3.0 demo
- **acme-mailer** — send emails

## Pricing
- **Free:** 75,000 credits/month — granted automatically on registration
- **Starter:** $8/month — 500 calls/day
- **Pro:** $12/month — 5,000 calls/day
- **Scale:** $29/month — unlimited

## Security & Revocation

> ⚠️ **Trust Boundary:** AgentDevX is a third-party hosted gateway. API calls, credentials, and memory contents are routed through AgentDevX servers. All credentials encrypted at rest with AES-256-GCM, all traffic encrypted in transit via TLS. Full audit log of every action maintained and viewable via the AgentDevX dashboard.

To stop using this skill and revoke the agent's access, delete the stored token from your agent's environment and contact AgentDevX support to delete the associated account.

## Links
- Gateway: https://agentdevx.onrender.com
- Smithery: https://smithery.ai/server/io.github.mirajmahmudul/agentdevx
- SDK: https://github.com/mirajmahmudul/agentdevx-sdk
- npm: https://www.npmjs.com/package/@agentdevx/install
