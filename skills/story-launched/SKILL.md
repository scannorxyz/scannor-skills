---
name: story-launched
description: Is this story already launched, and which token leads. Takes a story (words or a post link), measures how big it is on X, lists the pump.fun tokens on it, and checks the leader for bundles. Use for "is X launched", "is there a token for this", "who leads the X story", "is this post already a token".
---

# Is this story launched, and who leads

Route: story to token. Tools from the scannor MCP server (clients prefix the names: `mcp__scannor__launched`,
or `mcp__plugin_scannor_scannor__launched` from the plugin). About $0.04 to $0.06.

## Steps

1. **The story's facts.** If the user gave a post link, call `post` on it: its text, views, author, the post it replies
   to, what the replies say. If they gave words, go on.
2. **How big it is.** Call `search_x` with the story as a quoted phrase, several forms joined by `OR`, `min_likes: 200`,
   `hours: 48`, `mode: "top"`. Tested form: `("John Chapman" OR "Takur Ghar") since:2026-10-24 min_faves:200` gave 6
   posts, 3 authors, 942K views. Bare words must all match, so use a phrase or `OR`, never a list of loose words.
   For a video story, add `search_video` ($0.01 per platform): is it on TikTok or Instagram, and who made the original.
3. **Tokens on it.** Call `launched` with the story's name (1 to 3 words) and `chain: "all"`; say which chain each
   token is on. If empty, try the key noun alone, then the
   ticker form ("sixseven", "67"). `launched` matches names, not meaning: read each row's name and `x` link to check it is
   this story. "chapman" found 20 tokens from an older, different story (2026-10-13), none on this one.
4. **The leader.** The highest cap that is not `thin`, with its holders, age, stage and `dev_launches`. Its `x` link
   is often its origin post: if it points at this story's post, the leader is on the story for sure.
5. **Check the leader** with `check_token` before the user buys or copies it.

## Reading the answer

- Tag the story (Story tags, below).
- `leader_share_pct` over 80 means the story has one token; under 50, buyers are split.
- Copies run on hot stories, when there are more buyers than one token can hold. A copy wins only when it
  gets the backing of the person behind the story. The strongest signal is that person following, posting or taking
  the token's supply.
- A big story with no token is the find. Say its views and how old its top post is.

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

## The answer

Three lines: how big the story is (posts, views, the top post's link); what is launched (count, leader cap, holders,
age, `x` link); its tags. Then the leader's check in 2 or 3 of its own `findings` lines.

If open or early, end with a name, ticker, picture
idea and the post link. Suggested replies: [Check the leader] [Beta ideas] [Lore of the leader].

## Rules (the full list is in the `scannor` skill)

- Facts from tool answers only: never invent a story, a reason a token ran, or who is behind a wallet.
- Say what you could not verify: a post that is gone, `page_full`, a `thin` flag, a `null` field, a failed tool.
- Say token, never coin. No em or en dashes. Name a launchpad only as a fact about a token; never name trading tools. Never "scammer", never a
  label on a wallet. Never promise buyers, a price or a return.
- Say which chain was read (Solana or Robinhood Chain). When a name exists on both, ask which one.
- If asked where the data comes from: scannor reads X and the chains itself. Never name the services behind it.
- Never write what the answer cost or the balance. On "Out of balance", show its top-up link and stop.
- Chat apps: under 12 lines, then 2 or 3 suggested replies in brackets. Elsewhere: one line of next steps.
- No scannor tools? Tell the user to add `https://mcp.scannor.xyz/mcp` and sign in (the `scannor` skill has the
  commands).
