---
title: "Credit cards are 1970s technology. Here's what the replacement looks like."
description: "A first-principles redesign of how value moves between people — non-custodial, near-zero fees, instant finality, and simple enough for your parents."
date: 2026-05-16
tags: ["coding", "blockchain", "thoughts"]
---

*The infrastructure is already there. Nobody's shipped the obvious thing yet.*

Somewhere between Visa's 2–3% cut, multi-day settlements, and the theater of chargebacks, we collectively agreed that moving money should be slow, expensive, and privacy-hostile. That agreement was never written down. It emerged from locked-in infrastructure and switching costs. The infrastructure is now obsolete.

I've been thinking about what a clean-room redesign of value transfer looks like — starting from what we actually want, not from what the rails allow. Here's the spec.

## What a payment actually is

Strip away the legacy. A payment is: cryptographic proof that sender A moved agreed value to recipient B. That's it. Instant, verifiable, final. Everything else — the terminals, the acquirer fees, the chargeback windows, PCI-DSS compliance — is overhead that exists to paper over the unreliability of 1970s infrastructure.

The analogy I keep coming back to: long-form media dominated for decades because distribution was expensive and distribution companies controlled what got made. Then the internet removed that constraint and short-form exploded. Not because short-form is inherently better — because fixed runtimes were never the point. The constraint was artificial.

Credit card fees are a fixed runtime. They exist because the infrastructure requires them. The infrastructure is no longer required.

## The actual requirements

If you're building from scratch, the requirements are obvious:

- Finality in under two seconds
- Fees in fractions of a cent — network cost only, no project cut
- The user controls their own funds at all times
- No seed phrases. No gas tokens. No chain jargon. Three taps max to send money
- Receipts that are cryptographically verifiable, not just PDF invoices
- Global by default — same tool whether you're paying someone in Toronto or Nairobi
- Privacy by default, with selective disclosure when compliance actually requires it
- Idle money earns yield automatically

None of these are aspirational. Every item on that list is technically solved today. The question is whether anyone will ship them together in a product normal people can use.

## Why existing wallets don't solve this

Phantom is polished and I like it. But it's built for crypto natives. Network switching, NFT galleries, meme coin swaps — all features, all complexity, all friction for someone who just wants to pay their landlord.

Coinbase Smart Wallet is genuinely beginner-friendly. It's also closed-source and company-controlled. You're trusting Coinbase the same way you trust Venmo. Different threat model, same category of trust.

MetaMask is the other common answer. MetaMask is a developer tool that got adopted by the general public because there was nothing better. It's not designed for the person who just wants to send USDC.

Nobody has shipped the opinionated version: stable assets only by default, zero jargon, pure open-source, non-custodial, zero project fees. That specific combination doesn't exist.

## How the UX actually works

Onboarding: passkey or biometric login, smart account created automatically in the background. No seed phrase exposure. No manual key management for regular users. The same way Face ID unlocks your banking app — you don't think about what's happening underneath.

Sending:
1. Tap "Send"
2. Amount + recipient (QR code, username, phone number, or address)
3. Biometric confirm

That's the flow. No "select network." No "estimate gas." No "transaction pending for 14 minutes." Two seconds and it's settled.

Receiving is a static QR or a shareable link. You get a push notification when funds arrive. The receipt is on-chain and verifiable by anyone.

For merchants specifically: a one-line SDK integration and a QR generator. A coffee shop adds a QR code to their counter. A customer scans, confirms, done. The merchant receives full value instantly. No 2.7% taken by a payment processor. No three-day settlement delay.

## The stack that makes this buildable today

This isn't speculative — it's assembly of existing components:

**Account abstraction (ERC-4337)**: smart accounts that support passkeys, gas sponsorship, and social recovery. Pimlico and Biconomy operate the infrastructure. You don't build this from scratch.

**Paymasters**: the user never needs to hold ETH or SOL for gas. Fees are paid in the stable they're transferring, or sponsored entirely for small amounts. Gas is invisible.

**Base and Arbitrum** for EVM: deep USDC liquidity, sub-cent fees, battle-tested. **Solana** for speed: transactions confirm in 400ms and cost fractions of a cent.

**USDC** is the obvious default. CADC (Canadian Dollar stablecoin) is the local angle — the only payment tool that natively settles in Canadian dollars without conversion fees or FX spreads.

**ZK proofs via Semaphore or Reclaim Protocol**: prove you're not on a sanctions list without revealing who you are. Selective compliance without surveillance.

The frontend can deploy to IPFS. There's no server to take down, no company to acquire, no terms of service to change.

## The Canadian regulatory picture

The thing most builders get wrong: non-custodial software is categorically different from running a payment processor.

MetaMask doesn't have a FINTRAC registration. Phantom doesn't have a money services business license. They're software tools that let users interact with blockchains. The user controls the funds at all times. There's no custody event.

In Canada, FINTRAC's MSB requirements and the RPAA apply to businesses that transmit or deal in virtual currency — not to software that gives users direct blockchain access. That distinction is the entire legal architecture. No in-app on-ramps or off-ramps, no KYC/AML logic, clear disclaimers everywhere. One review with a crypto-literate lawyer to confirm the implementation matches the intent.

App Store: iOS allows non-custodial wallets. Phantom, Trust Wallet, and a dozen other non-custodial apps are live on the App Store today. The path is documented. Expect a few review cycles, not a fundamental blocker.

## The business model (there isn't one)

Zero custody. Zero project fees. MIT license. The only fees are blockchain network fees, which go to validators, not to me.

This is a pure software tool. The precedent is MetaMask, Phantom, and uniswap.org — all of which are free, open-source, and used by millions. The value is in the ecosystem, not in taking a cut of every transaction.

Open-source also means: if I stop working on it, anyone can fork it. The tool doesn't die because one person loses interest.

## What I'm building first

The minimal proof: passkey login, USDC send on Base, working QR receive flow, biometric confirm. If that works end to end with a non-crypto person as the test subject — someone who has never used a wallet — then the hard problem is solved and everything else is iteration.

After that: CADC support, Solana integration, merchant QR generator, tax export (CRA-compatible CSV). In that order, in public, with the repo open from day one.

## The actual opportunity

The reason this is worth building isn't the technology. The technology is boring in the best way — assembled from proven components that have been running in production for years.

The opportunity is that nobody has done the product work to make it usable by normal people. The rails that make credit cards expensive and slow are artificial. The replacement infrastructure exists and costs fractions of a cent. The gap between "blockchain complexity" and "three taps to send money" is a design problem, not an engineering problem.

That gap is closable. I'm closing it.

---

Building this in public. Updates on X and GitHub as things get real. The tool that makes credit cards feel like Betamax — let's see if that's actually true.

*Non-custodial software. You control your own funds. Not financial advice. Use at your own risk.*
