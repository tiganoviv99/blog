---
layout: post
title: "Ranking lessons I would re-learn the hard way"
date: 2025-11-16
tags: [ranking, recsys]
---

I have worked on ranking systems long enough to know which mistakes are
waiting for every team that builds one. Here are the lessons I would
re-learn the hard way if I started over, in the order the pain arrives.

## The baseline is your best teacher

Whatever you build, build it next to something simple: a popularity
ranking, a heuristic, a random shuffle even. The baseline tells you
whether your sophistication is adding anything. Most of my early "gains"
evaporated when measured honestly against a simple baseline.

## Position bias poisons everything you measure

Users click the first result because it is first. Train on raw clicks and
the model learns to reproduce your current order and call it success.
Correct for position bias in training, in evaluation, and in the A/B
metrics, or your offline numbers will be fiction.

## Features beat architecture, until they don't

For the first months, every win came from features and data quality. Then
the feature wins flattened, and only then did architecture start to
matter. The teams that invert this order burn months on elegant models
that lose to a better feature set.

## Diversity is a product decision

Identical recommendations are technically relevant and commercially
useless. How much diversity to allow, and what counts as diverse, is a
business call. Make it explicitly, tune it small, and watch it in the
A/B test rather than discovering it in user feedback.

## The long tail is the product

The head queries and popular items take care of themselves. The long tail
is where the ranking either shines or breaks, and it is exactly the part
that global metrics hide. Evaluate the tail separately, and ship for it
deliberately.

## The one metric that rules them all

If I had to keep a single number for a ranking system, it would not be
NDCG and it would not be revenue. It would be the fraction of sessions
where the user found what they came for, however we measure that:
conversion for transactional surfaces, completion for informational
ones. Everything else is a proxy, and proxies drift.

The point of naming that number is that it disciplines everything
else. Features, models, and diversity tunings all get judged against
it, and the judgments survive fashion changes in methodology. Methods
come and go; the question "did the user succeed" stays. Build your
ranking around that question, and most other decisions become
corollaries.

Ranking is a system, not a model

The model is one component among candidates, bias, cold start, diversity,
serving, and monitoring. The teams that treat ranking as a system ship
steady wins. The teams that treat it as a model chase leaderboards and
wonder why the business does not move.
