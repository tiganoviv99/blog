---
layout: post
title: "What to log before you even have a model"
date: 2026-04-16
tags: [logging, production]
---

Teams love to argue about models before they have data, and about
monitoring after they have incidents. The cheapest way to avoid both is to
log the right things from the very beginning, before the first model exists.

Here is the list I would set up on day one of any ML project.

## Log the request context

Every prediction should be logged with everything around it: who, what,
where, when, and which version of everything served it. User identifier,
item identifier, session, timestamp, model version, feature set version. If
a bad prediction surfaces later, this context is the difference between a
mystery and a reproduction.

## Log the inputs, not just the output

The features that went into the prediction are the most valuable log you
can have, and the one most teams skip. Without them, you cannot debug
train/serve skew, cannot reproduce the prediction, and cannot build a
training set from production traffic later. Logging inputs feels expensive
and is the best investment in the whole list.

## Log the output with its confidence

A prediction without a score is a verdict without an argument. The score
lets you build calibration, thresholds, and debugging tools later, when you
are not in a hurry. Log raw scores, not just the final decision.

## Log what the user did next

The prediction is only half the story; the outcome is the other half. Clicks,
purchases, corrections: log the feedback signal as close to the event as
possible. This is the label factory for every future model, and you cannot
build it retroactively.

## Log the pipeline state

Which data snapshot, which training run, which config. Models fail in
obscure ways, and the difference between a two-hour investigation and a
two-day one is usually whether the pipeline state was written down at
serving time.

## Storage costs and retention

Logging everything sounds expensive until you compare it with the cost
of not logging: the debugging time, the unreproducible incidents, the
training sets you cannot build. Still, retention deserves a policy.
Raw request logs can be heavy, so we kept raw for a short window and
aggregated, sampled, or feature-only versions for the long tail.

The rule we settled on: keep what you would need to reproduce an
incident for as long as incidents take to surface, and keep what you
would need to train a model for as long as models take to develop. The
numbers came out to weeks for raw logs and months for features, and the
policy was written down so storage debates stopped recurring.

The principle

Logging is a bet that the future version of you will be confused. The future
version of you always is. Make the bet, pay the storage cost, and build the
logging like it is part of the product, because it is.
