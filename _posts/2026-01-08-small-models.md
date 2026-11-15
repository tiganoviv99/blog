---
layout: post
title: "Small models, big pipelines: a year of ML without a GPU"
date: 2026-01-08
tags: [mlops, models]
---

There is a fantasy version of ML work where the blocker is always compute:
bigger models, more GPUs, longer training runs. There is a real version,
much more common, where the blocker is a broken join in a pipeline and the
model fits on a laptop.

I spent most of a year in the second version. It taught me more.

## The model is rarely the bottleneck

When we profiled where the time actually went, training was a footnote.
Data preparation, feature pipelines, evaluation, and debugging ate the
hours. Upgrading the hardware would have made the footnote slightly
smaller. Fixing the pipelines made the whole system faster.

## Constraints force boring excellence

Without the option of throwing compute at problems, we had to be careful:
sample wisely, cache aggressively, evaluate cheaply. The habits stuck.
Teams with unlimited compute often never learn them, and it shows in
their cloud bills and their debugging skills.

## Small models are easier to own

A model you can run anywhere is a model you can test anywhere. Reproducing
a failure locally, re-running an evaluation, experimenting with a feature:
all of it happens in minutes when the model is small, and in days when it
is not. Ownership is inversely proportional to model size.

## The 80/20 of capability

For most business tasks, a modest model with good data beats a large model
with mediocre data. We learned to exhaust the data and the process before
reaching for more parameters. The upgrade path, when it finally came, was
more valuable because the foundations were already solid.

## When to graduate

The small-model phase ends when the bottleneck is proven, not when the
budget arrives. The proof looks specific: evaluation gains that
plateau after a certain data size, or a real business task that a
small model measurably fails. Until that proof exists, more compute
mostly buys faster training of the same mistakes.

When we finally did graduate, the transition was easy precisely
because the foundations were in place: clean pipelines, solid
evaluation, reproducible runs. The new hardware accelerated an already
working system instead of papering over a broken one. That is the
order that scales, and the reverse order is how teams burn cloud
budgets without learning anything.

What I tell teams now

Start small, and only grow when the bottleneck is proven. Compute is
cheaper than ever, and discipline is as rare as ever. The year without a
GPU was not a limitation; it was training.
