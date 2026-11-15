---
layout: post
title: "The case against daily retraining"
date: 2026-05-08
tags: [mlops, production]
---

Somewhere along the way, "fresh model" became a synonym for "good model".
Teams retrain daily, weekly, or continuously, and feel virtuous about it.
I want to make the unfashionable case: most models do not need daily
retraining, and some are worse off for it.

## Freshness is not the same as improvement

A model trained every day is a model that changed every day, and change is
risk. The question is not "is the model fresh" but "does freshness pay".
We measured it: for most of our models, weekly retraining performed as well
as daily on the business metric, with a fraction of the incidents.

## Frequent retraining multiplies failure modes

Every training run can fail, and every deployment can degrade. Daily
retraining means a daily lottery. The pipeline itself became a source of
instability: a bad batch of data one evening, a subtly different feature
distribution the next morning. Stability has value that freshness culture
ignores.

## Slow features don't care about fast retraining

If the strongest features update weekly, retraining daily just re-fits the
model to stale data. The freshness of the weakest link matters, not the
frequency of the ceremony. We aligned retraining cadence with the cadence
of the data that actually moved the metric.

## Monitor first, retrain second

The trigger for retraining should be a signal: drift beyond a threshold,
business metric decay, a data update. A schedule is a proxy for a trigger,
and a bad proxy. We moved to event-driven retraining with a scheduled
safety net, and the noise level dropped immediately.

## The honest questions

Before you retrain daily, ask: what changed in the data since yesterday?
What is the smallest change we would act on? How often does the model
actually improve? If the answers are "not much", "large", and "rarely", you
are retraining for the feeling of progress, not for progress.

## What to do instead

The cycles you save by not retraining daily are real, and they are
best spent where the returns are higher. Audit the evaluation set for
leaks and staleness. Build the freshness checks on upstream tables.
Improve the monitoring on the business metric. Each of these moves the
needle more than another daily training run, and none of them adds a
new daily failure mode.

Retraining cadence should follow from data cadence and business need,
not from a feeling of diligence. Write the cadence down, attach a
trigger condition to it, and review it quarterly. The goal is a
pipeline that retrains exactly as often as it should, which is almost
never "every day, just in case".

Freshness is a tool, not a virtue. Train when the data says so, and spend
the saved cycles on evaluation and monitoring, where the real gains live.
