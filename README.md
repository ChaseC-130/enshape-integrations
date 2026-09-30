# Enshape assistant connections

Connection metadata and setup instructions for Enshape's hosted, proprietary MCP service. This repository contains no application or server source, credentials, or user data.

- Website and account setup: https://enshapeapp.com/integrations
- Remote Streamable HTTP endpoint: `https://enshapeapp.com/mcp`
- Official MCP Registry: https://registry.modelcontextprotocol.io/v0.1/servers/com.enshapeapp%2Fenshape/versions/1.0.0
- Support: https://enshapeapp.com/feedback
- Privacy: https://enshapeapp.com/privacy-policy
- Terms: https://enshapeapp.com/terms-and-conditions

## Connect your own account

Each person connects their own Enshape account from their own assistant. This MCP integration provides diary tools; it does not run model inference or route customer requests through the developer's personal agent account. Your assistant provider's plan and usage rules apply.

For Codex CLI:

```sh
codex mcp add enshape --url https://enshapeapp.com/mcp
codex mcp login enshape
```

For Claude Code:

```sh
claude mcp add --transport http --scope user enshape https://enshapeapp.com/mcp
claude mcp login enshape
```

For Claude web/mobile, add that URL in the account's custom connector settings and complete Enshape OAuth. Claude Code and web/mobile connections have separate authentication.

Eligible Gemini accounts can add that URL through Connected Apps → Custom apps. Google controls eligibility and available conversation modes. Review Google's notice and Enshape's account/scopes before approving.

The authorization window uses existing Google/Apple Enshape sign-in. Check the intended Enshape account, callback origin and diary permissions. Revoke a connection at https://enshapeapp.com/assistant-connections. Do not configure a shared developer account or paste Enshape session tokens into assistant configuration.

## Tools and example

- `get_meals`: read the connected account's private diary for an explicit local calendar date.
- `recent_foods`: retrieve up to 50 recent entries with saved nutrition.
- `log_meal`: save one explicitly requested private entry with per-serving nutrition and a request ID for safe retries.

Example: “Log two servings of oats for breakfast on September 29, 2026. Per serving: 150 calories, 5g protein, 27g carbs, 3g fat and 4g fiber; serving size 40g, from my food label.”

Provide all five nutrition values and the intended date/portion. Estimates need explicit approval and are labeled. Entries can be checked in the Enshape web or mobile diary. The MCP exposes no deletion, public-catalog publication or access to other accounts.

## Free and Premium allowances

Free Enshape accounts receive **20 diary tool calls per UTC day**, shared across every connected assistant. Each valid diary read, recent-food lookup or meal-save attempt consumes one call; retries also count. OAuth setup, initialization and tool discovery do not consume this allowance. Usage resets at midnight UTC and is visible in [Assistant connections](https://enshapeapp.com/assistant-connections).

Enshape Premium provides higher usage through the existing mobile app subscription. Manage or restore Premium while signed into that same Enshape account. Higher usage requires a server-verified subscription; client flags cannot bypass the limit. Premium remains subject to the overall **600 authenticated protocol requests per day per account**, including tool calls and protocol setup/discovery requests. No MCP tool initiates a purchase or processes a payment.

## Security and capacity

The service requires scoped OAuth with S256 PKCE and exact registered callbacks. Tokens are resource-bound; access expires after one hour and rotating refresh credentials have a 30-day absolute expiry. Only credential hashes persist. Refresh replay, connection revocation and account-session revocation invalidate access.

Authenticated protocol requests are capped at 120/minute and 600/day per account, plus 10,000/day for the shared service. Registration and unauthenticated traffic have separate quotas. MCP JSON bodies are limited to 64 KiB. These use the existing hosting and database, with no added model inference service.

## Availability

The remote service and official MCP Registry entry are live. Production natural-language Codex and Claude web tests verified diary reads, saving/retrying a synthetic entry without duplication, and Claude's recent-food tool. OpenAI and Claude directory review submissions remain in preparation.

iOS 26.5 Simulator Safari onboarding/sign-in layout was checked. Latest-iOS, native app synchronization and each voice mode still require device validation. Google AppFunctions' early-access registration is currently closed; no acceptance or directory approval is claimed.
