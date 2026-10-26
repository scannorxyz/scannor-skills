---
name: token-lore
description: The lore of a token, and whether it looks undervalued. Opens the token's links and origin post, reads the replies, counts who on X names its contract address, compares it with rival tokens on the same story, and checks it for bundles. Use for "what is the lore of X", "why is this token running", "is X undervalued", "is this token worth it", a pasted contract address.
---

# The lore of this token, and is it undervalued

Route: token to story. Tools from the scannor MCP server (clients prefix the names: `mcp__scannor__launched`,
or `mcp__plugin_scannor_scannor__launched` from the plugin). About $0.06.

## Steps

1. **The token.** If the user gave a ticker or name, call `launched` with it and `chain: "all"`, and take the right row (if several match,
   ask which, showing cap and age). Then `token` with the mint: links, `origin_post`, market numbers, what the graph
   knows about its dev and early wallets.
2. **The origin post.** Call `post` on `origin_post` (or the first post link in `links`). It returns the post, the post
   it replies to, and its top replies. If the answer is "That post is gone", say so and try the token's other post
   links: the SWORDCAT leader's origin account was suspended, and its metadata held a second post link (not read in the test).
   For a video meme, `search_video` with the story's words finds where it spread and whom captions credit as the
   original (the sword cat meme's credited `@sickmanai`).
3. **Who talks about it.** Call `attention` with the mint. It searches the contract address itself (tested: 20 posts
   in 4.7 hours for SWORDCAT, 17 authors, 6 with over 1,000 followers, 11 posts under 100 views). A ticker search mixes
   in bots shilling other addresses under the same ticker: never use one for attention.
4. **Rivals on the story.** Call `launched` with the story's name. Is this token the leader, a copy, or a beta?
5. **The check.** Call `check_token`. Bundles change the answer more than any story.

## Is it undervalued?

There is no single number. Report these and say which way each points:

- **Story reach per dollar:** the origin post's views (and its parent's) against the market cap. A story with 800K
  views on a $20,000 token points up; 150K views on $2.6M does not. Compare with the rivals from step 4, never alone.
- **Real buyers:** `organic_buyers_24h` against `traders_24h` (SWORDCAT: 111 of 4,811 traders) and
  `posts_under_100_views` against `posts`. Mostly bots means the volume is not people.
- **Backing:** does the person or account the story is about post the token, follow it or reply to its posts (read in
  `post` and `attention`)? The strongest signal; a copy wins only with it.
- **The dev:** `dev_launches` and `dev_graduated` (165 and 24 for the SWORDCAT dev): a dev with hundreds of launches
  and few graduations makes many tokens.
- **The check:** Block-0 share, linked holders, dev holding (from `check_token`'s `findings`).

Say "big story, small cap" only with the two numbers next to it, and call it a flag to look closer, not a prediction.
Thresholds will come from scannor's weekly outcome report; until then, compare within the scan.

## The answer

The lore in 3 lines, each with a source link: what the story is, where it started, who pushed it. Then the numbers
above in a short list, then one line: what looks strong, what looks weak, what could not be verified.

If the story has room for another token, offer to run the `beta` skill's ideas. Suggested replies: [Check holders] [Beta ideas] [Track the dev].

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
