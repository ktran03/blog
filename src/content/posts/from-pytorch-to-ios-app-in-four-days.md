---
title: "From 'what's a tensor' to a full ML pipeline in 4 days"
description: "How AI-assisted learning let me go from not knowing what a tensor is to training a custom model and shipping a full ML pipeline — over a single weekend."
date: 2026-04-12
tags: ["ai", "learning", "coding", "agents"]
---

It started on a Thursday with a simple question: *how does an AI model actually work?*

Not in a hand-wavy "it learns from data" way. I mean mechanically — what is happening inside the thing? I kept seeing PyTorch come up everywhere and had no idea what it was. So I opened up an LLM and started asking. One word kept blocking me: **tensor**. I didn't know what a tensor was. Five minutes later, I did.

## The drilling loop

The first answer it gave me on PyTorch — I understood maybe 40% of it. One word kept appearing: **tensor**. I didn't know what a tensor was. So I asked about tensors. Five minutes later, I got it. Back to PyTorch. Hit another term I didn't know. Drilled into that. Back to the overview. Repeat.

This is a different way to learn. It's not reading a textbook front to back. It's not sitting through a tutorial series for three weeks before you touch anything real. It's more like having a patient expert in the room who never gets tired of your questions and always knows exactly how much context you have.

Within a couple of hours, the whole picture clicked.

## The aha moment

At some point I asked the LLM to just explain what a model *is* at the lowest level. And it said: a model is essentially a collection of matrices — multi-dimensional arrays of numbers. Those numbers are what the model "knows." Training is the process of feeding inputs, checking how wrong the output was, and nudging those numbers slightly in the right direction. Do that millions of times with enough data, and the numbers converge to encode what we'd call truth.

That hit different.

Because isn't that exactly how a human brain works?

We start with almost nothing. We navigate the world, get feedback, and slowly our neurons wire and rewire. We don't consciously store "lessons" — we store *weights*, adjustments, patterns. The primitives are different (neurons vs. floating point matrices), but the concept is the same. A machine learning model is a brain — a digital, narrowly scoped one, but the same basic idea.

Once I saw that, the mysticism evaporated. There was no magic box anymore. Just math, feedback, and iteration.

## Then I remembered a project I'd been sitting on

I'd been wanting to build something around **Chữ Nôm** — the classical Vietnamese script used for centuries before the Latin-based alphabet took over. Most Vietnamese people today can't read it, which means a huge chunk of cultural and historical text is essentially locked away.

I had the idea of an app that lets you point your phone at a manuscript and get a translation. But every time I thought about starting, the barrier felt enormous. I didn't know ML. I didn't know Core ML. I didn't know anything about training a classifier. The starting friction felt like a wall.

After that Thursday afternoon of drilling, the wall wasn't there anymore.

## What I built by Sunday

I won't pretend it wasn't a full sprint. But here's what existed by Sunday evening:

**The ML pipeline** — fine-tuned EfficientNet-B3 with a custom classification head to recognize ~1,000 Chữ Nôm characters. Transfer learning from ImageNet weights, then trained on character image datasets. The model runs entirely on-device via Core ML, fast enough to classify in real time.

**The iOS app** — you photograph a manuscript page. The app detects character bounding boxes using Apple's Vision framework, crops each one, runs it through the model, and shows the character with its Quốc ngữ transliteration and English meaning. Confidence-based routing: high confidence shows immediately, low confidence escalates to Claude Vision API as a fallback.

**OTA model delivery** — models aren't bundled in the app binary. They're hosted on S3 and downloaded on demand. This means I can ship new model versions without an App Store update.

**A website** for the project.

Thursday to Sunday. Starting from: *what's PyTorch?*

The week after, I let my machine run phase 2 training — extending to ~7,000 Chinese Han characters and incorporating manuscript scan data I found. That's still going.

## What this actually means

This isn't a story about how smart I am. I'm not. A week earlier I didn't know what a tensor was.

It's a story about what the bottleneck actually is now. The bottleneck is no longer:
- Not knowing enough to start
- Spending months in tutorial hell before touching a real problem
- Getting stuck on a concept and losing momentum
- Nit-picking every line of code when the architecture is what matters

The bottleneck is: **do you have a clear enough problem to jump into?**

With an LLM in the loop, you can clear conceptual blockers in minutes instead of days. You can go from "I don't understand this" to "I understand this well enough to keep moving" in a single conversation. And because you're always moving — because you never get stuck long enough to get bored or discouraged — you reach somewhere significant before you run out of steam.

The other thing that helped: I created markdown docs as I went. Not as formal documentation. Just notes — what I learned, what decisions I made and why, what I still didn't understand. Those docs became context for future LLM sessions, and they kept me oriented in a project that was moving fast.

## The meta-lesson

I think a lot of people are still treating LLMs like fancy search engines. They are not. The right mental model is: an infinitely patient collaborator who can meet you exactly where you are and walk with you toward where you need to be.

You don't need to know everything before you start. You just need a problem worth solving and the willingness to drill into whatever's blocking you.

The starting friction isn't gone — you still have to care enough to begin. But the *sustaining* friction, the part that used to kill projects, is mostly gone. And that changes what's possible.
