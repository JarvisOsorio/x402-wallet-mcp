# Optional: Screwtape.ai spend clearance before `call_endpoint`

This wallet already enforces allowlists and spend caps. Some fleets also want an **independent allow/deny** on the seller before USDC leaves the wallet.

[Screwtape.ai](https://screwtape.ai) exposes prepaid Payment Guard clearance agents can buy themselves (x402 USDC on Base). Complementary to this MCP — not a replacement for local allowlists.

## Buy prepaid clearance (agent-payable)

```bash
# Returns HTTP 402 + exact payment requirements; settle with @x402/fetch / AgentKit
curl -i -X POST https://screwtape.ai/v1/packages/metered_spend_clearance/buy \
  -H 'content-type: application/json'
```

- **$29 / 5,000 clearances / 30 days**
- Machine catalog: https://screwtape.ai/pricing.json
- Agent guide: https://screwtape.ai/agents.md
- MCP: `{"mcpServers":{"screwtape":{"url":"https://screwtape.ai/mcp"}}}`

## Suggested agent flow

1. Discover / negotiate the paid endpoint as usual.
2. (Optional) Call Screwtape Payment Guard / payment-check for the seller URL + amount.
3. Pay only on `allow` / `pass`, then use this wallet’s `call_endpoint`.
4. Keep this package’s allowlist + daily cap as the hard local backstop.

Free try (no package): `POST https://screwtape.ai/v1/signup` then a few payment-check calls, or anonymous free tier per `/pricing.json`.

No dependency change required — agents that already speak x402 can buy and call Screwtape over HTTP/MCP alongside this server.
