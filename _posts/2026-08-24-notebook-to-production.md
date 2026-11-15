---
layout: post
title: "From notebook to production: the checklist I actually follow"
date: 2026-08-24
tags: [production, mlops]
---

A notebook is a hypothesis. Production is a commitment. The gap between them
is where most ML projects quietly die, and it is almost never because the
model was bad.

Here is the checklist I follow when a model graduates from exploration to a
real system. It is not glamorous, and that is the point.

## Reproduce with one command

If training requires five manual steps in a specific order, you do not have
training; you have a ritual. Everything must run from one entry point with
pinned data versions, a seed, and a written commit. I have wasted days on
"the numbers changed" mysteries that were just forgotten manual steps.

## Features must be computed twice: offline and online

The single most dangerous bug in ML systems is the train/serve skew: a
feature computed one way in training and another way at serving time. The
fix is to have one definition, shared code where possible, and explicit
parity tests that compare training features with production features on the
same keys. This test has caught more real bugs for me than all unit tests
combined.

## Test the features, not just the code

Unit tests on Python functions give a false sense of safety when the real
risk is in the data. Distribution tests on features, sanity ranges, null-rate
checks, and invariants like "no negative prices" protect you from the
failures that actually happen in production.

## Shadow mode before the A/B test

Run the new model alongside the old one, log everything, change nothing for
users. Shadow mode exposes latency problems, missing features at serving
time, and silent crashes before any user can feel them. It costs very little
and saves launches.

## Know your rollback story

Every model change needs a way back that does not require a heroic redeploy
at 2 a.m. A feature flag, a versioned artifact, a config switch: pick one and
practise using it. If your rollback is "someone will figure it out", you are
not ready to ship.

## Monitoring from day one

The launch is not the finish line. Data drift, upstream schema changes, and
traffic shifts start happening immediately. Basic monitors on input
distributions and on the business metric, with a clear owner and a clear
runbook, should go live with the model, not after it.

## The meta-rule

## Who owns the checklist

A checklist nobody owns is a list. On teams where this worked, one person
owned the productionisation checklist, reviewed it quarterly, and removed
items that stopped paying for themselves. That ownership mattered because
checklists rot: some items become automatic habit, others become obsolete,
and a few were never catching anything in the first place.

The goal is not maximal completeness; it is a small set of checks that catch
real failures, applied consistently. Our list shrank over time even as our
confidence grew. That is the healthy direction: the checklist gets shorter
because the habits got internalised, and the checks that remain are the ones
with scars behind them.

Everything on this list is about reducing the surface area of surprise. The
model itself will surprise you less than the environment it lands in. Prepare
for the environment, and the model will get a fair chance to be good.
