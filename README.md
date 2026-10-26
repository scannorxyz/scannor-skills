# scannor

What is spreading on X, TikTok and Instagram, which tokens are already on it, and whether a token is worth anything,
on Solana (every launchpad) and Robinhood Chain. Ask in plain words; scannor reads the posts and the chains and answers
with facts, never invented stories.

This repo is the scannor plugin: the scannor MCP server (`https://mcp.scannor.xyz/mcp`) and eight skills.

| Skill | Ask it |
| --- | --- |
| running-now | "What is running right now?" |
| story-launched | "Is the swordcat story launched? Who leads?" |
| check-token | "Check <token address>": bundles, dev holding, top 10, linked holders |
| token-lore | "What is the lore of <token>? Is it undervalued?" |
| big-name-touch | "Did these accounts touch anything today?" |
| beta | "Give me a beta for this story" |
| track-wallet | "Track <wallet>": its buys and sells |
| scannor | Anything else: devs, funders, wallets, attention |

## Install

**Claude Code**

```
/plugin marketplace add scannorxyz/scannor-skills
/plugin install scannor@scannor
```

Then `/mcp`, pick scannor, Authenticate.

**Codex**

```
codex plugin marketplace add scannorxyz/scannor-skills
codex plugin add scannor@scannor
codex mcp login scannor
```

**Cursor**: [Add scannor to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=scannor&config=eyJ1cmwiOiJodHRwczovL21jcC5zY2Fubm9yLnh5ei9tY3AifQ==), or add
`https://mcp.scannor.xyz/mcp` as an MCP server, then sign in when Cursor asks.

**claude.ai**: Settings, Connectors, Add custom connector, URL `https://mcp.scannor.xyz/mcp`.

**Telegram**: [@scannorxyzbot](https://t.me/scannorxyzbot).

Sign in with X, Google, email or a Solana wallet. Every account starts with $1 free.

## Pricing

Each call costs $0.01 or $0.02 from a prepaid balance (a full "what is running" scan is about $0.06). Your AI says what
a scan cost and what is left. Top up in SOL or USDC at [scannor.xyz](https://scannor.xyz).
