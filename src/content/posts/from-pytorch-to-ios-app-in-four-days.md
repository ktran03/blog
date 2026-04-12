---
title: "From 'what's a tensor' to a full ML pipeline in 4 days"
description: "The biggest barrier to building isn't knowledge — it's the feeling you don't know enough yet to start. Here's what happened when that barrier disappeared."
date: 2026-04-12
tags: ["ai", "learning", "coding", "agents"]
---

It started on a Thursday with a simple question: *how does an AI model actually work?*

Not in a hand-wavy "it learns from data" way. I mean mechanically — what is happening inside the thing? I kept seeing PyTorch come up everywhere and had no idea what it was. So I opened up Claude and started asking. One word kept blocking me: **tensor**. I didn't know what a tensor was. Three minutes later, I did.

I want to be upfront before going further: I didn't do this alone, and I'm not going to pretend otherwise. Claude was a co-builder throughout — explaining concepts, writing code, reviewing decisions, catching mistakes. What I brought was the problem, the direction, and enough curiosity to keep asking questions until things made sense. The point of this post isn't that I became an ML engineer in 4 days. It's about being empowered to build things you couldn't have built before — and realizing that the biggest barrier was never really knowledge. It was the feeling that you didn't know enough yet to start.

## The drilling loop

The first answer Claude gave me on PyTorch — I understood maybe 40% of it. One word kept appearing: **tensor**. So I asked about tensors. Three minutes later, I got it. Back to PyTorch. Hit another term I didn't know. Drilled into that. Back to the overview. Repeat.

This is a different way to learn. It's not reading a textbook front to back. It's not sitting through a tutorial series for three weeks before you touch anything real. It's more like having a patient expert in the room who never gets tired of your questions and always meets you exactly where you are.

Within a couple of hours, the whole picture clicked.

## The aha moment

I wanted more than a surface-level understanding — I wanted a deep intuitive picture of the whole thing. So I kept probing, kept asking follow-up questions, kept pushing until things connected. A picture emerged: a model is essentially a collection of matrices — multi-dimensional arrays of numbers. Those numbers are what the model "knows." Training is the process of feeding inputs, checking how wrong the output was, and nudging those numbers slightly in the right direction. Do that millions of times with enough data, and the numbers converge to encode what we'd call truth.

That hit different.

Because isn't that exactly how a human brain works?

We start with almost nothing. We navigate the world, get feedback, and slowly our neurons wire and rewire. We don't consciously store "lessons" — we store *weights*, adjustments, patterns. The primitives are different (neurons vs. floating point matrices), but the concept is the same. A machine learning model is a brain — a digital, narrowly scoped one, but the same basic idea.

Once I saw that, the mysticism evaporated. There was no magic box anymore. Just math, feedback, and iteration.

## Then I remembered a project I'd been sitting on

I'd been wanting to build something around **Chữ Nôm** — the classical Vietnamese script used for centuries before the Latin-based alphabet took over. Only around 100 people in the world can still read it fluently, which means centuries of cultural and historical text is essentially locked away.

I had the idea of an app that lets you point your phone at a manuscript and get a translation. There are a few other projects working in this space, but they're mostly academic — hard to access, not user friendly, no correction flywheel, require an internet connection, inference done server-side. That wasn't what I had in mind. But every time I thought about starting, the barrier felt enormous. I didn't know ML. I didn't know Core ML. I didn't know anything about training a classifier. The starting friction felt like a wall.

After that Thursday afternoon of drilling, the wall wasn't there anymore.

## What we built by Sunday

I won't pretend it wasn't a full sprint. And I want to be clear about what "built" means here: I drove the decisions — what to build, how it should work, what tradeoffs to make. Claude wrote a lot of the code, explained every piece I didn't understand, and pushed back when I was heading in the wrong direction. I reviewed, directed, and learned as we went. Here's what existed by Sunday evening:

**The ML pipeline** ([github](https://github.com/ktran03/nomlens-MLmodel)) — fine-tuned EfficientNet-B3 with a custom classification head to recognize ~1,000 Chữ Nôm characters. Transfer learning from ImageNet weights, trained on character image datasets. The model runs entirely on-device via Core ML. I understood the architecture conceptually; Claude helped translate that understanding into working PyTorch code.

**The iOS app** ([github](https://github.com/ktran03/nomlens-ios)) — you photograph a manuscript page. The app detects character bounding boxes using Apple's Vision framework, crops each one, runs it through the model, and shows the character with its Quốc ngữ transliteration and English meaning. Confidence-based routing: high confidence shows immediately, low confidence escalates to Claude Vision API as a fallback.

**OTA model delivery** — models aren't bundled in the app binary. They're hosted on S3 and downloaded on demand. As users correct misidentified characters, those corrections feed back into training. The model improves over time, and I can ship updated versions anytime — no App Store update required.

**[NomLens](https://nomlens.com)** — the project has its own site if you want to dig deeper.

Thursday to Sunday. Starting from: *what's a tensor?*

The week after, I let my machine run phase 2 training — extending to ~3,000 more Nôm characters, ~7,000 Chinese Han characters, and incorporating real manuscript scan data I found. That's still going.

## Learn fast, build as you go, fill in the gaps

The conventional path is: read theory, understand the system, then build something. That can take months before you touch anything real. What happened here was different — and faster.

I started with a novice understanding, picked up a bunch of concepts quickly through Claude, then jumped into building while I was still learning. The two happened simultaneously. But AI-assisted building moves fast — faster than you can fully absorb everything in real time. So naturally things blur past you. You make decisions you don't fully understand yet. You use a technique because Claude recommended it and it works, but you couldn't explain every detail of why.

And that's fine. Because now you go back and fill in those gaps — and it's a completely different experience than reading theory cold.

"Backpropagation" means something different when you've already watched it improve your model's accuracy in real time. "Temperature scaling" clicks faster when you've already seen what miscalibrated confidence scores do to a routing system you built. The mental scaffolding is already there. The gaps fill in faster, and they stick.

The conventional path keeps you in theory until you feel ready to build. The problem is you never quite feel ready, and the theory doesn't fully make sense until you've used it. So you wait to start until you understand it, but you can't fully understand it until you start. AI breaks that loop — you can learn and build at the same time, move fast, and circle back to solidify what you covered too quickly the first time.

I know way more about ML now than I did on Thursday. Not because I sat and read about it — because I built something with it, end to end, and now when I go deeper into the parts I don't fully understand yet, I already have the full picture in my head. That's a different kind of knowing.

## What this actually means

This isn't a story about how smart I am. I'm not. A week earlier I didn't know what a tensor was. And I want to be clear — I'm not an ML engineer now. I'm a novice in every sense of the word. There's a mountain of things I don't understand yet, and I'm under no illusion about that. But I'm learning, picking away at it, and I have something real to show for it.

The bottleneck used to be:
- Not knowing enough to start
- Spending months in tutorial hell before touching a real problem
- Getting stuck on a concept and losing momentum
- Not being able to write code fast enough to keep up with your own ideas

With Claude in the loop, those bottlenecks largely disappear. You can clear a conceptual blocker in minutes. You can go from "I don't know how to implement this" to working code in a single conversation. And because you're always moving — because nothing blocks you long enough to kill your momentum — you reach somewhere real before you run out of steam.

The other thing that helped: I created markdown docs as I went. Not formal documentation — just notes on what I learned, what decisions I made and why, what I still didn't understand. Those docs became context I fed back into Claude sessions, keeping continuity across a project that was moving fast.

## The meta-lesson

Most people have ideas they never act on. Not because they can't learn what's needed, but because the mountain of things they'd have to learn before feeling ready to start just looks too high. So they wait. And waiting becomes not starting. And not starting means the idea never happens.

AI flattens that mountain. Not by removing the learning — you still have to learn, and you should — but by being there for every gap, in real time, so the excuse to not start disappears. You don't need to know everything before you begin. You just need to begin, and let the understanding catch up as you go.

That's what changed for me. I wasn't waiting to feel ready anymore. I had a collaborator that could cover my gaps while I built up the knowledge to cover them myself. The permission to start was always there — I just needed something to make that obvious.

A lot of people are still treating LLMs like fancy search engines or autocomplete. That's not what this is. The right mental model is a collaborator — one that can explain, build, review, and push back, all at the level you're currently at. And that changes everything about what you can attempt.

The claim isn't "I learned ML in a weekend." The claim is: I had an idea I'd been sitting on because I didn't think I could get started, and now it exists. And I understand it far better than I would have if I'd spent months reading before touching any of it. That's what's different now. The things you've been waiting to know enough to build — you can probably start them today.
