---
layout: post
title: "The feed carousel that moved ARPU by 1%: the boring details behind a nice number"
date: 2026-10-20
tags: [recsys, ab-testing]
---

A personalised ranking model for the main feed carousel on a delivery app
lifted ARPU by 1% in A/B testing. The number sounds small. It is not: at
scale, a 1% ARPU lift is the kind of result that pays for a team.

But the headline hides where the time went. Almost none of it went into the
model.

## Candidate generation first, ranking second

You cannot rank what you cannot retrieve. Most of the engineering went into
candidate generation: what can even appear in the carousel, how many
candidates, how fresh they are, and how the system behaves when there are
only six restaurants left open at 2 a.m. A perfect ranker over a broken
candidate set is a perfect waste.

## Position bias follows you everywhere

Users click the first item partly because it is first. If you train on raw
clicks, the model learns to reproduce your current ranking. We had to correct
for position bias in the training objective and, more importantly, in how we
sampled negatives. Get this wrong and your offline gains are an artefact.

## Cold start is a business decision

New restaurants and new users have no history, and the defaults you choose for
them are policy, not science. We explicitly decided how much exploration to
allow, how to handle items with almost no data, and what the fallback ranking
looks like when personalisation is not available. These choices moved metrics
as much as any model change.

## Diversity and novelty are guardrails, not features

A carousel full of near-duplicates of one winning cuisine converts poorly and
looks broken. We added light diversity penalties and a novelty mechanism so
the feed felt alive. The trick was tuning them without destroying the
relevance signal. Small coefficients, watched carefully in the A/B test.

## The metric is ARPU, not clicks

Clicks are easy to move and cheap to fake. The experiment was judged on ARPU
with guardrails on session depth and conversion. This framing saved us from
shipping at least one variant that looked excellent on click-through and
terrible on revenue.

## Serving is part of the product

A ranking model nobody talks about at design time is the serving stack:
feature freshness, latency budgets, and fallbacks when the feature store is
slow. A carousel that takes 400 milliseconds to personalise is a worse
product than a slightly dumber carousel that loads instantly.

We made feature freshness explicit: some features were allowed to be minutes
old, others had to be current per request, and the fallback behaviour for
each tier was designed, not improvised. When the system degraded, it
degraded to a known state. That predictability mattered more to the business
than any single ranking gain, because it kept the feed alive under load. A
feed that survives peak hours earns more trust than one that wins
benchmarks.

The lesson from this project is a general one: in recommender systems, the
model is the smallest part of the product. Candidates, bias, cold start, and
the metric definition are where the work lives, and they are also where the
risk lives.
