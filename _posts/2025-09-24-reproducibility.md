---
layout: post
title: "Reproducibility: the boring insurance policy"
date: 2025-09-24
tags: [mlops, reproducibility]

Nobody gets promoted for making training reproducible. It is the
insurance policy of ML: invisible, slightly annoying, and invaluable
exactly when everything else goes wrong. Here is why it matters and
what the minimum viable version looks like.

## "It worked on my machine" is an admission

If training succeeds on one laptop and fails in the cloud, the system
does not work; it only sometimes works. Reproducibility is the property
that turns "sometimes works" into "works". Without it, every environment
change is a mystery and every new teammate is a hazard.

## Pin everything that can be pinned

Code version, data snapshot, dependencies, config, seed. All of it
written down, all of it committed. The goal is not perfection; it is
that a future engineer, starting from a clean machine, can reproduce
the last good model without asking anyone. We wrote a reproduction
runbook for every model and tested it quarterly.

## Data snapshots are worth more than code

Code changes slowly; data changes constantly. The most common source of
unreproducible results for us was not code drift but data drift: the
warehouse moved on, and the old numbers no longer existed. Snapshot the
training data, or at minimum pin the query and the timestamp. Data
without provenance is just numbers.

## The failure mode nobody mentions: legal

In regulated work, "how was this model trained" is not curiosity; it is
an audit question. The team that can answer it from artifacts answers in
an afternoon. The team that cannot answers in a month, or worse,
guesses. Reproducibility is compliance wearing casual clothes.

## The minimum viable version

One entry point, pinned inputs, a written runbook, and a quarterly test
that someone else can follow it. That is it. You do not need an
orchestration platform, just discipline.

## One thing to do this week

Do not build a platform. Do this instead: pick the model you would be
most afraid to lose, and try to reproduce it from artifacts on a clean
machine. Time yourself. Everything that blocks you is a reproducibility
debt, and most of it can be fixed in an afternoon: a pinned dependency
here, a data snapshot there, a missing config committed to the repo.

The quarterly habit is what matters, not the tooling. Reproduce one
model every quarter, fix what broke, and within a year your team will
have the property others buy platforms to fake: the ability to rebuild
their own work. That property is what protects you when people leave,
when the warehouse migrates, and when the audit email arrives.

Insurance feels like a cost until the incident. Reproducibility feels
like a cost until you need to rebuild a model from scratch, and then it
feels like the only thing standing between you and a very long week.
