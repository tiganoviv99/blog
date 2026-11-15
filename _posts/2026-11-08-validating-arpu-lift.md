---
layout: post
title: "How we validated a 0.5pp ARPU lift – and why the dashboard was not enough"
date: 2026-11-08
tags: [ab-testing, ranking]
---

A ranking experiment once showed a beautiful curve on the dashboard two weeks
after launch: ARPU up, engagement up, every segment green. The A/B test later
killed it. Not because the model was bad, but because the dashboard was
answering a different question than the experiment.

That experience shaped how I think about validating model improvements now.
Here is the discipline I stick to.

## Define the metric before the model

The time to decide what "better" means is before the first training run, not
after the results arrive. In our case the primary metric was ARPU, with
revenue-per-session and conversion as secondary. Engagement alone was never a
success criterion: a ranking that makes users scroll more while buying less is
worse, not better.

Write the metric definition down, with the exact SQL and the exact window, and
make everyone agree. Otherwise the post-hoc discussion becomes "let's pick the
metric where we won".

## Trust only randomized comparison

A pre/post comparison is not evidence. Seasonality, promotions, and external
traffic spikes all move your metrics more than most model changes do. If you
cannot randomize, you cannot conclude.

The pipeline we eventually built raised ARPU by a validated 0.5 percentage
points in A/B testing. The word "validated" carries all the weight there: the
same model shown as a before/after chart looked twice as good.

## Watch novelty effects

New ranking surfaces often get a short honeymoon: users click unfamiliar
items, sessions lengthen, and then the effect decays as they adapt. Two weeks
is usually too early to call a winner. We read results weekly but only acted
after watching the cumulative curve flatten.

## Segment before celebrating

A model can win overall and lose badly in one segment, and that segment might
be the one you care about. We always checked a handful of fixed cuts: new vs
returning users, heavy vs light buyers, top categories. A 0.5pp ARPU win that
is negative for new users is a design problem, not a victory.

## The dashboard is for monitoring, the test is for decisions

Dashboards are great at showing you that something changed. They are terrible
at telling you why. The experiment framework, with pre-registered metrics and
guardrails, is the only place where a decision should be made.

## Guardrails during the test

A ranking experiment can degrade in ways no dashboard shows: empty carousels,
duplicate items, one category dominating the first screen. So we added
guardrails to the test itself: automatic checks on result diversity, on
null-result rates, and on per-segment revenue. If a guardrail tripped, the
experiment paused and the on-call engineer looked at it before users did.

Guardrails are not bureaucracy. They are the difference between discovering
a regression in a weekly review and discovering it in a support ticket. They
also made the team calmer about shipping: when you know the safety nets are
real, you run more experiments, and more experiments mean more learning.

If there is one habit to adopt from this post, make it this one: never
announce a result you have not seen survive a randomized test.
