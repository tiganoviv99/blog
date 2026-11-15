---
layout: post
title: "Feature stores are not a silver bullet: what we actually needed"
date: 2026-06-24
tags: [features, mlops]
---

Every ML platform discussion eventually reaches the same sentence: "we need
a feature store". Sometimes that is true. Often it is a solution looking for
a problem, and the real need is smaller and more boring.

Here is what we actually needed, in the order we needed it.

## One place to see a feature's definition

Most of our pain was not feature storage. It was feature meaning. When a
feature called "price_band_7d" broke, the engineer had to open three
repositories to learn what it was supposed to be. A feature store would not
have fixed that; a documented registry would.

We added a lightweight registry: name, owner, definition, source table,
refresh cadence, and where it was used. A page, not a platform. That single
page ended more arguments than any infrastructure we ever bought.

## Train/serve consistency beats storage elegance

The worst bug class in ML is the same feature computed differently offline
and online. A feature store can prevent this, but so can shared code, a
parity test, and a rule that features enter production only through one
path. We started with the rule and the test. They cost a day and caught real
skew within the first week.

## Freshness tiers, not a universal cache

Different features have different freshness needs. Availability should be
per-request; a popularity score can be a day old. We made the tiers explicit
and stored features accordingly: hot features in the serving layer, warm
ones in the database, cold ones in the warehouse. This was 90% of the value
of a feature store, achieved with tools we already had.

## Point-in-time correctness is a warehouse discipline

Reproducing training data as it looked on a given date matters, and it is
mostly about upstream tables keeping history. We made the warehouse keep
versioned snapshots and wrote features as pure functions of those snapshots.
Once that was true, backfills became reruns, and the urge to buy a feature
store quietly disappeared.

## When to actually buy one

None of this means feature stores are useless. When you have dozens of
models, hundreds of features, and multiple teams who need the same
definitions served consistently, a feature store earns its keep. The
tell that you are ready is not a slide about scale; it is concrete
pain: the same feature being computed three times, skew bugs recurring
across teams, point-in-time correctness demanded by auditors.

We were not ready for years, and the decision to not buy one was
correct every time we re-evaluated it. The infrastructure you build
before the discipline exists becomes legacy on day one. Build the
discipline, document the registry, prove the pain, and then let the
platform decision make itself.

The takeaway

A feature store is a fine tool once the registry, the parity test, the
freshness tiers, and the snapshots exist. Before that, it is a very
expensive shelf for a problem you have not yet defined. Build the discipline
first, and you may find the platform was never the bottleneck.
