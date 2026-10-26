---
name: big-name-touch
description: Did a big name touch anything. Reads what the X accounts the user names (Elon, a founder, a celebrity) posted or replied to lately, opens the posts they reacted to, and checks which tokens are on each. Use for "did Elon post anything", "did a big account reply to a meme", "any big name touches today", "what did <handle> post".
---

# Did a big name touch anything

Route: watcher. Tools from the scannor MCP server (clients prefix the names: `mcp__scannor__launched`,
or `mcp__plugin_scannor_scannor__launched` from the plugin). About $0.03 to $0.06 for a list of 10.

## Steps

1. **Which accounts.** scannor keeps no list: the user picks. If they have not named any in this chat, ask: "Which X
   accounts should I watch? Up to 20." Reuse their list for the rest of the chat.
2. **What they did.** Call `big_names` with `accounts` and `hours: 12` (24 for "today"). It costs $0.01 per 10
   accounts; Elon and other very busy accounts get their own search (he filled 18 of 20 posts on a shared page), so
   8 accounts plus Elon is $0.02. It runs the tested query `(from:a OR from:b ...)` with a time window.
3. **Find the touches.** A touch is a post or reply about a meme, a character, an animal, a picture, a phrase or a
   ticker. Skip politics, business and replies to friends. A reply's text is often only an emoji or a link
   (Elon's SWORDCAT reply was "@paranoidream ❤️"; Trump's posts were only links): call `post` on it, which also reads
   the parent post. The parent is the story.
4. **Tokens on each touch.** If `tokens_linking` is above 0, the graph already knows tokens that link that post. Else
   call `launched` with the subject in 1 to 3 words and `chain: "all"`. Tag each (below).
5. **Before the user acts:** `check_token` on the leader.

## Story tags

Give each story every tag that fits: a story can be taken and running at once. The leader is the biggest token on it
that is not `thin`. The numbers are starting values; scannor's weekly outcome report will tune them.

- **open:** no live token on it. Say how big the story is (the post's views).
- **early:** the leader is under 6 hours old and under $50,000.
- **running:** the leader is up 20% or more in the last hour (`change_1h_pct`).
- **accumulation:** the leader's holders grew 2% or more in the last hour while its price moved less than 10% either
  way, and `net_buyers_24h` is above 0. Skip it for tokens under an hour old: their holder change means nothing.
- **taken:** the leader is over $500,000 or graduated.
- **crowded:** 5 or more tokens on it in 24 hours (`saturation_24h.launched`).
- **fading:** the leader is down 20% or more in the last hour, or its holders fell 2% or more.
- **dead:** tokens were launched on it but none is alive (`tokens` empty, `dropped_dead` above 0).
- **fake:** the biggest token is flagged `thin` (its cap or market is not real). Say so, and use the next one.

## Worth knowing

- A big account or brand doing something (a follow, reply, quote, ad, a ticker in a bio) starts many runners.
- The strongest signal is the person a token is named after following it, posting it or taking its supply: then the
  story is theirs. A copy wins only when it gets that backing.
- Seen in the test: Elon replied "❤️" to a sword cat video on 2026-10-24 at 14:37 UTC. 6 tokens linking Elon's post
  were launched within 72 seconds; 31 hours later all 6 sat near $3,000. The token that already led the story
  (launched 2026-09-28) ran to $3M, per a curator's recap. Here the touch lifted the existing leader, not the copies.
- Not in v1: new follows. Say so if the user asks who a big account followed.

## The answer

One block per touch: who, what (one line, with the link), when (UTC), its views, then the tokens on it (leader cap,
holders, age, how many launched since the touch) and its tags. If nothing launchable was touched, say that in one
line: it is a real answer.

Where a touch is open, end with the name, ticker,
picture idea and the post link. Suggested replies: [Check the leader] [Open the post] [Beta ideas].

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
