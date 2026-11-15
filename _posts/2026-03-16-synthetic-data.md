---
layout: post
title: "Synthetic data: the good, the bad, and the quietly broken"
date: 2026-03-16
tags: [data, cv]
---

When real data is scarce or expensive, synthetic data looks like a miracle.
Sometimes it is. Sometimes it quietly teaches your model things that are
not true, and you only find out in production. Here is what years of
generated training data taught me.

## The good: scale and coverage

Synthetic data shines at coverage. You can generate lighting conditions,
angles, and edge cases that would take months to collect naturally. In a
self-checkout project, generated variations of the same product under
different distortions did more for robustness than any architecture change.
Scale that would cost a fortune to label arrives almost for free.

## The bad: the simulation gap

Generators have a style, and the model learns it. We saw models that were
excellent on generated images and mediocre on real ones, because the
generator's world was too clean. The fix was mixing: synthetic data as a
supplement, never a replacement, with real data always present in
training and evaluation.

## The quietly broken: label leakage

A generator often encodes the label in a way the model can exploit: a
slightly different background per class, a fixed camera angle, a watermark
of the rendering pipeline. The model gets great accuracy by cheating, and
the cheat disappears in production. We audited generated sets for
spurious correlations before trusting any gains.

## Evaluation must stay real

No model trained on synthetic data should be evaluated on synthetic data.
The evaluation set has to come from the real world, ideally from the exact
distribution the product faces. When we skipped this, every metric lied
upward.

## Choosing a generator is choosing a bias

Not all generators distort reality equally. We evaluated candidates the
same way we evaluate models: on the real evaluation set, never on the
generated one. A generator whose output improved the real-set metrics
stayed; a generator whose output only improved generated-set metrics
went, regardless of how pretty its samples were.

The practical test was simple and brutal: train on generated data
alone, evaluate on real data, and look at the gap. A large gap meant
the generator was teaching its own world, not ours. The best generator
we found still had a gap, but a small one, and the mixing ratio with
real data closed it. Perfection was never the bar; honesty about the
gap was.

The practical recipe

Generate for coverage, not for volume alone. Mix real and synthetic with
the ratio dictated by the evaluation. Audit the generator for leakage.
And always ask: does the synthetic data contain the thing the model must
learn, or just a shadow of it? If it is only a shadow, collect ten real
examples before generating ten thousand fake ones.
