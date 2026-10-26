---
name: check-token
description: Check a Solana or Robinhood Chain token before buying or copying it. Reads its launch on chain: Block-0 share, what the dev bought and holds, the top 10, holders linked by a shared funder or one transaction, new wallets, and what scannor already knows about those wallets. Use for "check this token", "is X bundled", "who holds this", "is this safe", a pasted contract address with "check".
---

# Check this token

Route: token check. Tools from the scannor MCP server (clients prefix the names: `mcp__scannor__launched`,
or `mcp__plugin_scannor_scannor__launched` from the plugin). 3 cents (`check_token` 2, `token` 1).

## Steps

1. Call `check_token` with the mint and `token` with the same mint (name, cap, holders, age, links), in parallel.
2. Read `findings` first: they are the bundle map's own sentences, with the ones worth a second look first
   (`look: true`). Quote them as they are; they are facts about the token, never about a person.
3. Then the numbers: `stats.block0` (wallets, % bought, % they hold now), `stats.dev` (bought, holds),
   `stats.top10`, `stats.linked` (wallets, groups, % they hold), `stats.fresh` (new wallets, % they hold),
   `stats.wash` (seconds where Block-0 sold and others bought it right back).
4. `graph.wallets`: holders scannor has seen before, with how many launches each was early in and how many ran past
   $1M. This is the part no other check has. Name the wallet (short form, first 4 and last 4) and the numbers.
5. If `stats.covered` is false, the read stopped before slot 50 on a busy launch: say the early numbers are a floor.

## How to read it (the playbook)

- Assume every new token is bundled. The question is how much is still held, and by whom.
- Block-0 is the launch's own block: only wallets ready before the launch buy there. Block-0 wallets that still hold
  a large share can sell into every buyer.
- Linked holders (a shared funder or one transaction) are one owner in practice: add their shares together.
- New wallets (first used within a week of launch) holding many tokens can be one person spreading supply.
- A dev who sold everything is common and says little alone; a dev holding over 5% is the bundle map's own
  second-look line.

## The answer

The token in one line (name, cap, holders, age). Then the second-look findings, then the numbers that matter in a short
list, then what the graph knows. End with one line: what a buyer should know first.

If one holder stands out (biggest linked group, or a wallet with a history in `graph.wallets`), offer to profile it
with the `track-wallet` skill. Suggested replies: [Profile top wallet] [Track the dev] [Lore of this token].

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
