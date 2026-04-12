---
title: "Claude Code after 30 days: what actually changed in my workflow"
description: "Past the honeymoon phase — what stuck, what didn't, and the thing I didn't expect."
date: 2026-04-10
tags: ["claude", "coding", "ai"]
---

I've been using Claude Code as my primary coding tool for about a month. Long enough for the novelty to wear off and for real patterns to emerge.

Here's what I actually found.

## What I expected

I expected Claude Code to be a better autocomplete. A smarter Copilot. Something that would fill in boilerplate faster and let me focus on the interesting parts.

That's not what it is.

## What it actually is

Claude Code is a collaborator that can read and reason about your whole codebase. The difference matters more than it sounds.

With Copilot, you're completing sentences. With Claude Code, you're having a conversation about a system. You can say "I want to refactor the auth flow to remove the session state" and it actually understands what that means across multiple files. It can tell you what will break, what the tradeoffs are, and then do the work.

That said, it's not magic. It makes mistakes. It sometimes confidently does the wrong thing. The skill isn't just prompting — it's knowing when to trust the output and when to read it carefully.

## What changed

**I read more code, not less.** Counterintuitively, using Claude Code made me engage more with the codebase. Because I'm reviewing what it produces rather than writing every line, I spend more time reading and understanding than I used to. That's been net positive.

**Small tasks got faster.** Tests, type annotations, refactors with a clear goal — these are genuinely much faster. I'm not spending mental energy on the structural stuff, so I can think more clearly about the logic.

**Large, ambiguous tasks got harder.** If you don't know exactly what you want, Claude Code will confidently build you the wrong thing. The bottleneck shifted from "can I implement this" to "do I know what I actually want." That's probably how it should be.

## The thing I didn't expect

I started writing better specs before coding. Because I've learned that if I hand Claude Code a vague task, I get a plausible-but-wrong result. So I now write clearer problem statements before touching the keyboard. Which means I'm thinking more before coding.

That might be the most valuable change.

## Should you try it

If you're a software engineer who writes code every day — yes, try it. Not because it's a productivity multiplier in every scenario, but because it changes how you think about writing code. That's worth understanding even if you decide it's not for you.

But don't expect it to replace thinking. It hasn't for me. It's changed what I think *about*.
