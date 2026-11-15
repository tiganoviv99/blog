---
layout: post
title: "Airflow backfills: the part nobody documents"
date: 2025-11-08
tags: [airflow, mlops]
---

Every Airflow tutorial shows a happy little DAG running forward in time.
Real life is mostly the other direction: the definition changed, the
upstream table got fixed, the feature needs rebuilding for the last three
months. Backfills are the part nobody documents, and they are where teams
learn whether their pipelines are actually solid.

## Dates are parameters, never assumptions

The moment a task computes "yesterday" inside its own logic, backfilling
becomes archaeology. Every task should take the execution date as an
explicit parameter and behave the same whether the date is yesterday or
ninety days ago. We enforced this with a review rule and a linter check,
and it saved every backfill that followed.

## Outputs are partitions, never appends

A task that appends to a shared table cannot be safely re-run. A task
that replaces a dated partition can be re-run forever. We rewrote our
jobs to partition by date and made "replace the partition" the default
pattern. Backfills went from incidents to routine.

## Backfills need a budget and a blast radius

Rebuilding a year of features can saturate the warehouse and starve the
production jobs that are still running. We scheduled backfills in chunks,
off peak hours, with explicit resource limits, and watched the production
DAGs like hawks. A backfill that takes down today's features is a
self-inflicted incident.

## Verify before you trust the backfill

A backfill that completes is not the same as a backfill that is correct.
Row counts per day, spot-checked values, a comparison against the
previous version: a five-minute verification caught errors that would
otherwise have poisoned the next training run.

## The backfill checklist

Before pressing the button, five checks. One: every task takes dates as
parameters, with no hidden "yesterday". Two: every output replaces a
dated partition; nothing appends to a shared table. Three: the backfill
runs in chunks, off peak, with resource limits. Four: row counts and
spot values are verified against the previous version before anyone
trusts the result. Five: the owning team knows it is running, because a
surprise backfill is an incident wearing a costume.

The checklist is the boring version of expertise. Follow it, and
backfills stop being events and become operations. The day a backfill
is as unremarkable as a deploy is the day the pipeline earned its
reliability.

The real lesson

Backfills are a test of your pipeline's fundamentals: parameterisation,
partitioning, idempotency. If backfilling is painful, the problem is not
the backfill; it is the design. Fix the design, and the painful days stop
being painful and start being boring, which is exactly how they should
be.
