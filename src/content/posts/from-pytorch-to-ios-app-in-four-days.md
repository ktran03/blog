---
title: "From 'what's a tensor' to a full ML pipeline in 4 days"
description: "The biggest barrier to building isn't knowledge — it's the feeling you don't know enough yet to start. Here's what happened when that barrier disappeared."
date: 2026-04-12
tags: ["ai", "learning", "coding", "agents"]
---

It started on a Thursday with a simple question: *how does an AI model actually work?*

Not in a hand-wavy "it learns from data" way. I mean mechanically — what is happening inside the thing? I kept seeing PyTorch come up everywhere and had no idea what it was. So I opened up Claude and started asking.

I want to be upfront: I didn't do this alone. Claude was a co-builder throughout — explaining concepts, writing code, reviewing decisions, catching mistakes. What I brought was the problem, the direction, and enough curiosity to keep asking until things made sense. The point of this post isn't that I became an ML engineer in 4 days. It's about being empowered to build things you couldn't have built before — because with AI alongside you, filling in the gaps in real time, the knowledge barrier breaks down.

## The drilling loop

The first answer Claude gave me on PyTorch — I understood maybe 40% of it. One word kept appearing: **tensor**. I didn't know what a tensor was. So I asked. Three minutes later, I got it. Back to PyTorch. Hit another term I didn't know. Drilled into that. Back to the overview. Repeat.

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

**The ML pipeline** ([github](https://github.com/ktran03/nomlens-MLmodel)) — fine-tuned EfficientNet-B0 with a custom classification head to recognize ~11,000 Chữ Nôm characters. Transfer learning from ImageNet weights, trained on character image datasets. The model runs entirely on-device via Core ML. I understood the architecture conceptually; Claude helped translate that understanding into working PyTorch code.

**The iOS app** ([github](https://github.com/ktran03/nomlens-ios)) — you photograph a manuscript page. The app detects character bounding boxes using Apple's Vision framework, crops each one, runs it through the model, and shows the character with its Quốc ngữ transliteration and English meaning. Confidence-based routing: high confidence shows immediately, low confidence escalates to Claude Vision API as a fallback.

**OTA model delivery** — models aren't bundled in the app binary. They're hosted on S3 and downloaded on demand. As users correct misidentified characters, those corrections feed back into training. The model improves over time, and I can ship updated versions anytime — no App Store update required.

**[NomLens](https://nomlens.com)** — the project has its own site if you want to dig deeper.

## Under the hood

A few technical decisions worth calling out.

**The class imbalance problem.** 461 Chữ Nôm-specific characters have zero real handwriting data anywhere in the world — they exist only as font renders. Meanwhile the 511 Han characters have ~576 real handwriting samples each from the CASIA database. Left unchecked, the model would see Han characters representing 99.4% of training data and learn to essentially ignore Nôm characters entirely. The fix is `WeightedRandomSampler` — each class gets a sampling weight inversely proportional to how many samples it has, so every class appears at roughly equal frequency in every training batch regardless of raw sample count. Without this, the whole Nôm side of the model would be useless.

**Transfer learning from ImageNet.** EfficientNet-B0 starts from weights pretrained on ImageNet — it already knows how to detect edges, curves, strokes, and shapes from a million photos. The classification head is replaced with a new one mapping to Nôm character labels, and the whole network is fine-tuned on character data. The pretrained backbone means we didn't need to learn basic visual features from scratch — we just needed to map existing feature detectors to a new vocabulary of characters. It's why strong accuracy is achievable on a relatively small dataset.

**Temperature scaling.** A model's raw confidence scores aren't calibrated out of the box. When it says "91% confident," that number doesn't necessarily reflect the actual probability of being correct. This matters a lot for NomLens because the entire routing system — accept immediately, flag for review, escalate to Claude Vision — depends on those confidence thresholds being meaningful. Temperature scaling is a post-training calibration step: fit a single scalar parameter on a held-out calibration set that scales the raw logits before softmax. The result: Expected Calibration Error dropped from 0.0887 to 0.0075. In plain terms — when the model says 90% confident, it's actually right about 90% of the time. The routing thresholds aren't arbitrary anymore.

---

A note on how this actually got built: I'm a 10+ year iOS engineer — that's my vertical. And yet the iOS code here was largely not written by me line by line. Same with the Python pipeline, where I have no prior background at all. In both cases I directed the agent, reviewed what it produced, pushed back when something was off, and steered the decisions. What I didn't do was nitpick every variable, every loop, every piece of logic.

That was intentional. If I'd stopped to hand-write every line — even on the iOS side where I could, or the Python side if I wanted to — I'd have gotten so bogged down in implementation details that I'd have lost the thread of what I was actually building. The goal was a working product, not a perfectly hand-crafted codebase. Staying at the level of decisions and direction is what let me move fast enough to see the whole thing come together before losing momentum.

On the held-out test split (real handwritten samples), the model hits 97.6% top-1 accuracy with ~1.4% falling back to Claude Vision. On actual manuscript photos in the field — aged paper, ink degradation, varying scan quality — my rough estimate is 75%+, though I don't have hard numbers there yet. Closing that gap is the point of phase 2 training with real manuscript data.

Thursday to Sunday. Starting from: *what's a tensor?*

The week after, I let my machine run phase 2 training — extending to ~3,000 more Nôm characters, ~7,000 Chinese Han characters, and incorporating real manuscript scan data I found. That's still going.

## Learn fast, build as you go, fill in the gaps

The conventional path is: read theory, understand the system, then build something. That can take months before you touch anything real. What happened here was different — and faster.

I started with a novice understanding, picked up concepts quickly through Claude, then jumped into building while I was still learning. The two happened simultaneously. But AI-assisted building moves fast — faster than you can fully absorb everything in real time. So naturally things blur past you. You make decisions you don't fully understand yet. You use a technique because Claude recommended it and it works, but you couldn't explain every detail of why.

And that's fine. Because now you go back and fill in those gaps — and it's a completely different experience than reading theory cold.

"Backpropagation" means something different when you've already watched it improve your model's accuracy in real time. "Temperature scaling" clicks faster when you've already seen what miscalibrated confidence scores do to a routing system you built. The mental scaffolding is already there. The gaps fill in faster, and they stick.

I know way more about ML now than I did on Thursday. Not because I sat and read about it — because I built something with it, end to end, and now when I go deeper into the parts I don't fully understand yet, I already have the full picture in my head. That's a different kind of knowing.

## The meta-lesson

This isn't a story about how smart I am. I'm not. A week earlier I didn't know what a tensor was. I'm not an ML engineer now — I'm a novice in every sense of the word, with a mountain of things still to understand. But I'm learning, picking away at it, and I have something real to show for it.

Most people have ideas they never act on. The mountain of things you'd need to learn before you could build them looks too high. So you wait. And waiting becomes not starting. And not starting means the idea never happens.

AI doesn't remove that mountain — you still have to climb it. But it climbs alongside you, filling in the gaps in real time, so you're never stuck at the base waiting to feel ready. The bottleneck used to be:

- Not knowing enough to start
- Spending months in tutorial hell before touching a real problem
- Getting stuck on a concept and losing momentum
- Not being able to write code fast enough to keep up with your own ideas

With Claude in the loop, those bottlenecks largely disappear. You can start now, learn as you go, and actually get somewhere before you run out of steam.

The claim isn't "I learned ML in a weekend." The claim is: I had an idea I'd been sitting on because I didn't think I could get started, and now it exists. And I understand it far better than I would have if I'd spent months reading before touching any of it.

The things you've been waiting to know enough to build — you can probably start them today.
