---
layout: post
title: "Cold start: the problem that quietly decides your recommender's fate"
date: 2026-05-24
tags: [recsys, ranking]
---

Nobody quits a product because the recommendations for their tenth visit were
bad. People quit because the first visit was bad. Cold start is not a corner
case in recommender systems; it is the product's first impression, and it
deserves more attention than the personalised model itself.

## New users: the first session is all you have

A new user has no history, no preferences, and no patience. Whatever we show
first determines whether there is a second session. We built explicit
defaults for the first visit: popular and fresh items, tuned by region and
time of day, with light personalisation the moment the first signal arrived.

The goal was not perfection. It was not being obviously wrong.

## New items: without exposure, nothing works

A new product has no interaction data, so collaborative signals cannot rank
it. If you do nothing, new items never enter the loop and never gain data.
We allocated a small, explicit share of impressions to exploration, watched
the conversion of those impressions, and let the winners graduate into the
regular ranking.

Exploration is a budget, not a switch. Spend it deliberately.

## The fallback is a product surface

When personalisation is unavailable or degraded, users see the fallback
ranking, and they judge it like any other part of the product. We treated
the fallback as a first-class experience with its own evaluation, not as a
default that nobody owned.

## Signals come faster than you think

A single click, a scroll stop, a favourite: these arrive within minutes, not
weeks. The systems that won at cold start were the ones that consumed early
signals quickly. Batch pipelines that updated preferences once a day lost
the first session; near-real-time updates won it.

## Measuring cold start

Cold start only improves when it is measured, and the right measure is
session-level, not model-level. The numbers that told the truth were
first-session conversion, first-week retention, and the share of new
items that earned their first meaningful interaction within days of
appearing. Model metrics could not see any of this.

We also compared the first session against the tenth, explicitly. The
gap between them is the price your cold start extracts, and shrinking
that gap became the stated goal of the project. It was a goal everyone
understood, including the product managers, which is rare and
valuable. Name the gap, watch it, and every cold-start decision gets
easier.

The lesson

Cold start is where recommender systems earn their users or lose them
forever. Put your best defaults there, give new items a fair chance, and
treat every fallback as a designed experience. The personalised model can be
average and the product still works. The first session cannot.
