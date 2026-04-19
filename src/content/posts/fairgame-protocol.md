---
title: "I'm building a wagering protocol where cheating is mathematically impossible"
description: "Not 'provably fair' in the marketing sense. Actually impossible. Here's the cryptography behind it and why it's finally buildable."
date: 2026-04-18
tags: ["coding", "blockchain", "thoughts"]
---

*Not "provably fair" in the marketing sense. Actually impossible.*

Every major online gambling platform has had a cheating scandal. Superuser accounts. RNG manipulation. Insider access. Players keep coming back anyway because there's never been a real alternative.

I've been thinking about this for a while and I think the alternative is now actually buildable. So I'm building it.

It's called FairGame Protocol. Here's the idea.

## The problem with "provably fair"

Most online casinos now claim to be "provably fair." What they mean is: after the fact, you can verify the outcome using a seed they publish. In theory.

In practice, the server generates that seed. The server controls it. A superuser with backend access can see it before you commit your bet. You're trusting a policy, not a mathematical guarantee.

The difference between "we promise we didn't cheat" and "we couldn't have cheated even if we wanted to" is the entire product.

## What actually makes it impossible

The foundation is a commit-reveal protocol. It's not a new idea — the cryptography behind it is from 1979. But nobody has built a clean, usable product on top of it for wagering.

Here's how it works for a coin flip:

1. Before the game, both players independently generate a secret on their own device
2. Each player hashes their secret and sends only the hash to the blockchain — the secret never leaves their device
3. The game resolves, both players reveal their secrets
4. The contract XORs the two secrets together to produce the outcome

Neither player can bias this. To manipulate the result you'd need to know the other player's secret at commit time. Which is impossible — it was generated on their device and never transmitted.

The hash is safe to publish publicly. Reversing SHA256 is computationally impossible. The contract verifies each reveal matches its committed hash before paying out.

The key thing: **there is no server involved in determining the outcome.** The blockchain is the referee. The math is the rulebook.

## It's not just coin flips

The same primitive — commit-reveal to generate a shared random seed — works for any game involving randomness or hidden information.

For card games, the shared seed determines the shuffle. Neither player controls it. After the game both players reveal their seeds and anyone can recompute every card dealt to verify the deal was honest.

During play, cards stay hidden using SRA commutative encryption — the same cryptographic technique from the original 1979 Mental Poker paper. Your hand is encrypted with your key. Your opponent literally cannot read it. Not because of a policy — because decrypting it requires your private key.

Every move is signed with the player's wallet key. If there's a dispute at the end, the signed move history is the evidence. Forging it requires breaking elliptic curve cryptography.

## The architecture that makes this sustainable

FairGame is a protocol, not an app. The base layer handles escrow, commit-reveal, and settlement. Game resolvers plug into it. Each is independently immutable once deployed.

Adding a new game means deploying a new resolver. The base layer never changes. The money logic never changes.

This matters for trust. "We just burned the upgrade key" is a real event — it means nobody, including me, can ever modify the contract. That's a trust milestone you can point to. Most platforms can never make that claim because their business model requires keeping the ability to modify things.

## Why now

A few things converged.

Solana transaction fees are fractions of a cent. On Ethereum, gas fees would make small bets economically absurd. On Solana the fee is invisible.

The wallet UX — Phantom, Backpack — is finally normal enough that non-technical people use it. "Connect wallet" is a familiar action.

The cryptographic primitives are all solved problems. Commit-reveal is textbook. The work is assembly and UX, and that's a fast problem now — especially building with AI assistance, which is how I'm approaching this the same way I built NomLens.

## What I'm building first

The coin flip. Not because it's interesting on its own — it's the simplest possible proof that the stack works. Two wallets, a wager, a mathematically fair outcome, automatic payout.

If that works end to end on mainnet with real money, everything else is just a resolver on top.

After that: Open Bet (any two people bet on any outcome via a shareable link, settled by oracle), then Poker (biggest card game globally, provably fair poker is a real headline in a market full of cheating scandals).

## The part I keep coming back to

People have tried versions of this before. Virtue Poker. PokerDAO. Others. They're all dead.

The consistent failure mode wasn't the cryptography — it was the product. Built by crypto people who didn't think about what players actually need. Getting to the first hand required understanding MetaMask, gas fees, and the protocol itself just to sit down.

The crypto has to be invisible. Connect wallet, pick a wager, game starts. The math happens in the background. Players who want to understand it can read the docs. Players who just want to play never need to.

That's the thing the previous attempts got wrong. The technology worked. The product didn't.

---

I'll be building this in public and posting updates here. The only online wagering platform where cheating is mathematically impossible. Let's see if that's actually true.
