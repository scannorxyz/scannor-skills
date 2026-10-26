---
name: running-now
description: What is starting to run right now. Finds the stories spreading on X and the tokens already launched on each (Solana and Robinhood Chain), so a trader or launcher sees which stories are open, early, running or taken. Use for "what is running", "what is viral", "what should I buy or launch now", "any narratives today", "what did I miss".
---

# What is running now

Route: story to token. Tools from the scannor MCP server (clients prefix the names: `mcp__scannor__launched`,
or `mcp__plugin_scannor_scannor__launched` from the plugin). A full scan is about 6 cents.

## Steps

1. **What spreads.** Call `viral` with `window: "6h"` (or `"24h"` when the user says "today" or "yesterday"). Add
   `media: true` when the user wants memes: memes are pictures and videos. Tested queries the tool runs:
   `min_faves:20000 since:<date> -filter:replies lang:en` (24h) and
   `min_faves:3000 since_time:<unix> -filter:replies filter:media lang:en` (6h).
2. **What is trading** (when the user buys rather than launches): `trending` with `window: "1h"` lists the launchpad
   tokens with the most real buyers. Match them to the stories from step 1.
3. **What already ran.** Call `recaps` with `days: 1`. Each line is a token that ran and the curator's reason. Use them
   as context: which kinds of story are paying this week.
4. **Pick up to 5 launchable stories** from `viral`. Launchable means it can be a token name and a picture: a
   character, an animal, a phrase, a person doing something odd, news with a name. Skip sports scores, ads and
   arguments with no character in them. Name each story in 1 to 3 words, the way its token would be named.
   Posts with `tokens_linking` above 0 are already launched: say so without a `launched` call.
5. **What is launched on each.** Call `launched` with the story words and `chain: "all"`. If it finds nothing, try one other form (the key
   noun alone: "takur" for the Takur Ghar story). Then tag each story (below).
6. **Only when asked, or before the user buys or copies a leader:** `check_token` on at most 2 leaders.
7. **Optional market mood** (when the user asks how the market is): `search_x` with
   `(pump.fun OR pumpfun) min_faves:300 lang:en -filter:replies`, `hours: 24`. Report the mood in one line. Its top
   posts often promote other launch sites: never name them.

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

A big account or brand doing something starts many runners, so run the `big-name-touch` skill too when the user
wants the whole picture.

## The answer

A table, one row a story: story, the post (views, link), tokens on it, the leader (cap, holders, age), its tags.
Then two lines: the best story for the user and why, in numbers.

Example from the test of 2026-10-25: the John Chapman story (Takur Ghar, 864K views) had no token: open. "six seven"
had a $3.96M leader with 39 holders, flagged `thin`: the number is not a market.

When a story is open, end with the name, ticker,
picture idea and the post link. Suggested replies: [Check the leader] [Beta ideas] [Big names today].

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
