---
layout: post
title: "A/B testing when you don't have traffic"
date: 2026-04-08
tags: [ab-testing, metrics]
---

A/B testing advice assumes you have users to spare. Thousands of them, and
weeks to watch. But plenty of ML work happens in products where the traffic
is small, the stakes are high, and the standard playbook does not apply.
Here is what actually works when the traffic is thin.

## Know your minimum detectable effect

With small numbers, the honest answer to "will this test be conclusive" is
often no. Calculate the sample size needed for the effect you would care
about. If the number is larger than your monthly traffic, an A/B test is
the wrong tool, and knowing that early saves weeks of pretending.

## Use pre-post design with care

When you cannot randomise, a careful before/after comparison is sometimes
the best available evidence. It is weak evidence, and you should treat it
that way: watch seasonality, hold a control period, and never present it as
proof. Weak evidence beats no evidence, but only if everyone is honest
about the weakness.

## Consider staggered rollout

Launching to 20% of traffic instead of 50% does not create more traffic,
but it does create a comparison, and it limits the damage radius. For small
products, staged rollouts with explicit checkpoints are often more
practical than classic split tests.

## Find a proxy metric with more signal

If the business metric moves once a month, find a faster proxy that moves
daily: engagement with the feature, completion rate, error rate. The proxy
does not replace the real metric, but it shortens the feedback loop
dramatically and makes iteration possible.

## Interrupted time series, honest maths

When a change ships everywhere at once, an interrupted time series can
detect a visible break in the metric trend. It is noisy and easily gamed by
seasonality, but combined with a holdout logic it is a legitimate part of
the small-traffic toolkit.

## Holding yourself honest

The biggest risk in low-traffic work is not a wrong conclusion; it is
an overstated one. So we wrote confidence levels into every finding:
what the evidence supports, what it does not, and what would change our
mind. "We are fairly sure this helps, and we will re-check in a month"
is a legitimate outcome. "The test was flat, so we shipped anyway" is
not.

Writing honesty down also changed planning: when evidence was thin, we
favoured changes that were cheap to reverse, and reserved the
expensive, irreversible ones for moments when the numbers were
genuinely solid. In low-traffic products, reversibility is a
substitute for statistical power, and it is the best substitute
available.

The mindset

Small traffic forces humility about evidence. The teams that succeed there
combine several weak signals, say explicitly how confident they are, and
build systems where mistakes are cheap to reverse. That last part matters
most: when proof is expensive, make rollback trivial.
