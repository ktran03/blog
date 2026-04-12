---
title: "From 'what's a tensor' to a full ML pipeline in 4 days"
description: "How AI-assisted learning let me go from not knowing what a tensor is to training a custom model and shipping a full ML pipeline — over a single weekend."
date: 2026-04-12
tags: ["ai", "learning", "coding", "agents"]
---

It started on a Thursday with a simple question: *how does an AI model actually work?*

Not in a hand-wavy "it learns from data" way. I mean mechanically — what is happening inside the thing? I kept seeing PyTorch come up everywhere and had no idea what it was. So I opened up Claude and started asking. One word kept blocking me: **tensor**. I didn't know what a tensor was. Five minutes later, I did.

I want to be upfront about something before going further: I didn't do this alone, and I'm not going to pretend otherwise. Claude was a co-builder throughout — explaining concepts, writing code, reviewing decisions, catching mistakes. What I brought was the problem, the direction, and enough curiosity to keep asking questions until things made sense. That collaboration is the whole point of this post.

## The drilling loop

The first answer Claude gave me on PyTorch — I understood maybe 40% of it. One word kept appearing: **tensor**. So I asked about tensors. Five minutes later, I got it. Back to PyTorch. Hit another term I didn't know. Drilled into that. Back to the overview. Repeat.

This is a different way to learn. It's not reading a textbook front to back. It's not sitting through a tutorial series for three weeks before you touch anything real. It's more like having a patient expert in the room who never gets tired of your questions and always meets you exactly where you are.

Within a couple of hours, the whole picture clicked.

## The aha moment

At some point I asked Claude to just explain what a model *is* at the lowest level. A model is essentially a collection of matrices — multi-dimensional arrays of numbers. Those numbers are what the model "knows." Training is the process of feeding inputs, checking how wrong the output was, and nudging those numbers slightly in the right direction. Do that millions of times with enough data, and the numbers converge to encode what we'd call truth.

That hit different.

Because isn't that exactly how a human brain works?

We start with almost nothing. We navigate the world, get feedback, and slowly our neurons wire and rewire. We don't consciously store "lessons" — we store *weights*, adjustments, patterns. The primitives are different (neurons vs. floating point matrices), but the concept is the same. A machine learning model is a brain — a digital, narrowly scoped one, but the same basic idea.

Once I saw that, the mysticism evaporated. There was no magic box anymore. Just math, feedback, and iteration.

## Then I remembered a project I'd been sitting on

I'd been wanting to build something around **Chữ Nôm** — the classical Vietnamese script used for centuries before the Latin-based alphabet took over. Most Vietnamese people today can't read it, which means a huge chunk of cultural and historical text is essentially locked away.

I had the idea of an app that lets you point your phone at a manuscript and get a translation. But every time I thought about starting, the barrier felt enormous. I didn't know ML. I didn't know Core ML. I didn't know anything about training a classifier. The starting friction felt like a wall.

After that Thursday afternoon of drilling, the wall wasn't there anymore.

## What we built by Sunday

I won't pretend it wasn't a full sprint. And I want to be clear about what "built" means here: I drove the decisions — what to build, how it should work, what tradeoffs to make. Claude wrote a lot of the code, explained every piece I didn't understand, and pushed back when I was heading in the wrong direction. I reviewed, directed, and learned as we went. Here's what existed by Sunday evening:

**The ML pipeline** ([github](https://github.com/ktran03/nomlens-MLmodel)) — fine-tuned EfficientNet-B3 with a custom classification head to recognize ~1,000 Chữ Nôm characters. Transfer learning from ImageNet weights, trained on character image datasets. The model runs entirely on-device via Core ML. I understood the architecture conceptually; Claude helped translate that understanding into working PyTorch code.

**The iOS app** ([github](https://github.com/ktran03/nomlens-ios)) — you photograph a manuscript page. The app detects character bounding boxes using Apple's Vision framework, crops each one, runs it through the model, and shows the character with its Quốc ngữ transliteration and English meaning. Confidence-based routing: high confidence shows immediately, low confidence escalates to Claude Vision API as a fallback.

**OTA model delivery** — models aren't bundled in the app binary. They're hosted on S3 and downloaded on demand. This means I can ship new model versions without an App Store update.

**[NomLens](https://nomlens.com)** — the project has its own site if you want to dig deeper.

Thursday to Sunday. Starting from: *what's a tensor?*

The week after, I let my machine run phase 2 training — extending to ~7,000 Chinese Han characters and incorporating manuscript scan data I found. That's still going.

## What this actually means

This isn't a story about how smart I am. I'm not. A week earlier I didn't know what a tensor was. And I want to be clear — I'm not an ML engineer now. I'm a novice in every sense of the word. There's a mountain of things I don't understand yet, and I'm under no illusion about that. But I'm learning, picking away at it, and getting somewhere.

The story is really about what the bottleneck is now. It used to be:
- Not knowing enough to start
- Spending months in tutorial hell before touching a real problem
- Getting stuck on a concept and losing momentum
- Not being able to write code fast enough to keep up with your own ideas

With Claude in the loop, those bottlenecks largely disappear. You can clear a conceptual blocker in minutes. You can go from "I don't know how to implement this" to working code in a single conversation. And because you're always moving — because nothing blocks you long enough to kill your momentum — you reach somewhere real before you run out of steam.

The other thing that helped: I created markdown docs as I went. Not formal documentation — just notes on what I learned, what decisions I made and why, what I still didn't understand. Those docs became context I fed back into Claude sessions, keeping continuity across a project that was moving fast.

## The meta-lesson

A lot of people are still treating LLMs like fancy search engines or autocomplete. That's not what this is. The right mental model is a collaborator — one that can explain, build, review, and push back, all while meeting you exactly where you are.

What changes is the ratio of thinking to doing. You spend more time on the problem — what to build, why it matters, whether the approach makes sense — and less time on the mechanics of implementation. That's not laziness. That's leverage.

I didn't learn ML in a weekend. But I learned enough to build something real, with a collaborator that filled the gaps I hadn't closed yet. And now those gaps are smaller than they were on Thursday.

That's what's different now.
