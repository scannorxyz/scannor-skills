---
name: track-wallet
description: Profile a Solana or Robinhood Chain wallet and track it (tracking is Solana only for now) when the user says so. Shows its early buys, the launches it was early in and how they did, its funder, what X says about it, and what scannor already knows; then watches its buys and sells. Use for "who is this wallet", "track this wallet", "what did my wallets buy", "stop tracking", a wallet address from a check.
---

# Track this wallet

Route: wallet. Tools from the scannor MCP server (clients prefix the names: `mcp__scannor__launched`,
or `mcp__plugin_scannor_scannor__launched` from the plugin). `wallet_profile` is $0.01; tracking costs $0.01 an alert (each buy or sell of the wallet), from the balance; `tracked` is free.

The chain side of these tools is specified, not yet tested against scannor's keys. The X side is tested: searching the
SWORDCAT dev's address on X found 0 posts.

## Steps

1. **Profile first.** Call `wallet_profile` with the address. Read: `early_in` (launches where it bought within 60 s of
   the create) and `early_tokens_ran_past_1m`, `funder` (and whether it is `busy`, an exchange), `x_posts`, and
   `graph` (`shares_funder_with`: other wallets with the same funder).
2. **Write what the numbers say,** never a label. "Bought in the first minute of 4 launches; 3 ran past $1M. Funded
   from one wallet that also funded 2 other early buyers. Nothing on X about this wallet." The user decides what that
   means. Never call a wallet an insider, a dev or a person's name unless a tool answer says so with a source.
3. **Ask before tracking.** Offer it, with the label you would use and the price: $0.01 for each buy or sell it
   makes. Only on a yes, call `track_wallet` with `wallet` and `label`. When the balance runs out, tracking stops and
   `tracked` lists the wallet under `stopped`.
4. **Activity.** `tracked` shows recent buys and sells and `together` (tokens 2 or more tracked wallets bought).
   In Claude Code and Codex the user asks for it; in Telegram each trade arrives as a message.
5. **Stop** with `track_wallet` and `stop: true`.

Private by design: scannor never shows who else tracks a wallet or how many do. Do not guess.

## The answer

The wallet (short form, first 4 and last 4) and its numbers in 3 to 5 lines, then the question "Track it?".
After tracking: "Tracking 3 wallets, $0.01 an alert." For activity: one line a trade (time UTC, buy or sell, token, SOL),
`together` first.

Suggested replies: profile [Track it] [Check its last token] [No]; activity [Check this token] [Stop tracking].

## Rules (the full list is in the `scannor` skill)

- Facts from tool answers only: never invent a story, a reason a token ran, or who is behind a wallet.
- Say what you could not verify: a post that is gone, `page_full`, a `thin` flag, a `null` field, a failed tool.
- Say token, never coin. No em or en dashes. Name a launchpad only as a fact about a token; never name trading tools. Never "scammer", never a
  label on a wallet. Never promise buyers, a price or a return.
- Say which chain was read (Solana or Robinhood Chain). When a name exists on both, ask which one.
- If asked where the data comes from: scannor reads X and the chains itself. Never name the services behind it.
- End with the cost and the balance from `_billing`. On "Out of balance", show its top-up link and stop.
- Chat apps: under 12 lines, then 2 or 3 suggested replies in brackets. Elsewhere: one line of next steps.
- No scannor tools? Tell the user to add `https://mcp.scannor.xyz/mcp` and sign in (the `scannor` skill has the
  commands).
