---
layout: post
title: "Ranking 1,600 categories: lessons from replacing a heuristic with a model"
date: 2026-11-12
tags: [ranking, recsys, mlops]
---

For a long time the category pages of a big retail site were ranked by a
hand-tuned heuristic. It was a list of rules: popularity first, then price
band, then margin, with a few exceptions nobody fully remembered. It worked
well enough to ship, which is exactly why it was hard to replace.

We replaced it with a learned ranker and added roughly $1.5M of monthly net
revenue across 1,600+ categories. Here is what actually mattered.

## Keep the heuristic as your baseline, not as your enemy

The first instinct is to delete the old logic the day your model beats it
offline. Resist. The heuristic is a documented, reproducible baseline that
behaves predictably in every corner case. When the model underperforms on some
category, the heuristic gives you the answer to the question "what were we
doing before" without archaeology.

## Offline metrics lie gently

NDCG on held-out clicks looked great three weeks before the launch. The A/B
test was more modest. The reason is position bias: users click top items
because they are on top. Your training labels are shaped by your current
ranking, so offline metrics always flatter the model that agrees with the
current order.

We learned to treat offline metrics as a filter, not a promise: a model must
beat the heuristic on NDCG, on revenue-weighted NDCG, and on a few
category-level slices. After that, the decision moves to the A/B test.

## Features were 80% of the work

The model was the easy part. The hard parts were price band encoding that
survives promotions, availability signals that do not flicker, seasonality
interactions (a category can have three "seasons" inside a month), and
popularity features that decay smoothly instead of in weekly cliffs.

Every one of these required a pipeline, a backfill, and an argument with
someone about definitions.

## Ship in two stages

We first deployed the model in shadow mode: it scored real traffic, logged
everything, changed nothing. That exposed latency surprises, missing features
at serving time, and categories where the model had almost no training data.
Only after two weeks of clean logs did we start the A/B test, with guardrails
for zero results, cold items, and inventory limits.

## What we would do differently

The long tail deserves more attention than we gave it. Categories with
little traffic barely moved the global metric and were easy to ignore, but
they add up. Today I would build a dedicated evaluation slice for low-traffic
categories and decide explicitly what "good enough" means there, instead of
letting the global number speak for them.

I would also invest earlier in the debugging tools around the ranker. When a
category ranking looks wrong, you want to see the features and the score for
each item in one place. We built that eventually; it should have been there
from the first shadow run.

## The lesson

Ranking problems are 20% model and 80% plumbing. The model gets the credit;
the pipelines, logging, and guardrails do the work. If you are starting a
similar project, budget your time accordingly.
