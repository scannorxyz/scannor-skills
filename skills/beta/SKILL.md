---
name: beta
description: Give me a beta for X. Takes a runner, finds the betas and copies already launched on its story with their caps, and writes 3 launch ideas tied to real facts of the story, each checked for name clashes. Use for "beta for X", "copy of X", "what is the next X", "ideas riding X", "what can I launch off X".
---

# Give me a beta for X

Route: story to idea. Tools from the scannor MCP server (clients prefix the names: `mcp__scannor__launched`,
or `mcp__plugin_scannor_scannor__launched` from the plugin). About 5 to 8 cents.

A beta is a new token that rides a runner's story from another angle (the runner's cat with a sword, a beta's cat in
armor). A copy is the same story again. Both are ideas built on a real story; neither is a new story.

## Steps

1. **The runner.** Call `launched` with its name and `chain: "all"` (or `token` with its address). Take the leader: cap, holders, age, its
   `x` link. If the user means a story with no token yet, use the `story-launched` skill instead.
2. **Its facts.** Call `post` on the leader's origin post (or the post that made it run, from `recaps` or
   `big_names`). The picture, the words, the character: every idea must come from these.
3. **Betas already out.** Call `attention` on the leader's mint and read `other_mints_named`: shillers post betas next
   to the runner's address. Then `launched` on 2 or 3 beta forms of the story (the character plus another item, the
   same item on another animal). Tested: `attention` on SWORDCAT named "CAT IN ARMOR"; `launched("cat in armor")` found
   it at $22,862 with 354 holders, and 4 more tokens on the same idea within 40 minutes.
   Do not search X for "beta": tested, 0 of 20 posts said it; the page was ticker chatter.
4. **How big betas got.** There is no reliable average. Show the betas already out on this story (step 3) with their caps: that is the real evidence. Tool
   caps are today's, not the top; a recap line ("hit $3m") gives the top when there is one.
5. **Three ideas.** Each: name, ticker, one line on the angle, the fact from step 2 it rests on, a picture idea. Then
   call `launched` on each name: drop or rename any that is already taken by a live token.

## Worth knowing

- Copies run on hot stories, when there are more buyers than one token can hold. A copy wins only when it gets the
  backing of the person behind the story.

## The answer

The runner in one line (cap, top, age). Betas already out (name, cap, holders, age). The 3 ideas,
each with its name check. End with, for the user's
pick, the name, ticker, picture idea and the origin post link.
Suggested replies: [Launch idea 1] [More ideas] [Check the runner].

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
