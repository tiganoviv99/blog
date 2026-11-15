---
layout: post
title: "Ten Airflow DAGs in production: rules that keep me asleep at night"
date: 2026-10-28
tags: [airflow, mlops]
---

At some point I looked at the Airflow UI and counted more than ten DAGs my
team maintained: feature collection, training triggers, monitoring jobs,
database updates. Each of them ran daily. Each of them had failed at least
once in a way that taught me something.

Here are the rules I now treat as non-negotiable.

## Idempotency is not optional

Every task must be safe to re-run, including after a partial failure. If your
feature job appends to a table, make it replace a partition. If it writes
files, make the output path deterministic. The moment a task depends on
"nothing else touched the table", you have built a time bomb with a schedule.

## Make backfills boring

Backfilling is not an emergency procedure; it is routine maintenance. Dates
as parameters, deterministic output partitions, and no dependence on
"today" being a specific weekday make a backfill a two-minute operation
instead of a two-day incident. I learned this the hard way after a
feature definition changed and we had to rebuild three months of history.

## Prefer explicit triggers over wall-clock hope

Tasks that depend on another team's pipeline should wait on a sensor or a
dataset readiness check, not on a scheduled start time. Schedules drift,
upstream jobs slip, and a two-hour sleep in a DAG is just a flaky test
disguised as orchestration. Whenever possible, we waited for the data to
exist rather than assuming it would.

## Alerts must point at the owner, not the dashboard

A DAG failure alerting to a general channel is noise; nobody owns noise. Every
DAG had a declared owner, alerts went to that owner, and a failing DAG was
either fixed or paused. Nothing rots a team's trust in monitoring faster than
a red square that stays red for a week because "someone will look at it".

## Log enough to debug without re-running

When a training job fails at step 37 of 40, you want to know which input
changed, what the data looked like at that step, and how far the job got.
Counts, checksums, and row-level samples in the logs have saved me more hours
than any clever code. Re-running a heavy job to reproduce a failure is a
luxury you do not always have.

## The DAG that watches the DAGs

Finally, we added a small monitoring DAG whose only job was to check the
health of the others: schedules running on time, durations within normal
bounds, key tables fresh. A pipeline that monitors the business metrics is
good. A pipeline that monitors itself is what lets you sleep.

## The part I still find hard

The hard part of DAGs is not writing them; it is keeping the dependency
graph honest as the team moves fast. Someone adds a task that depends on a
table produced by a job nobody owns. Someone else schedules a backfill over
a weekend when the source system is down for maintenance. The failure modes
are social, not technical.

The only defense I have found is making the graph visible: a diagram, a
convention for naming datasets after their producer, and a review rule that
no DAG may read from an undocumented source. It slows things down by a few
minutes per change and prevents hours of confusion later. Slower at the
merge, faster in the war room.

Airflow is just a scheduler with a pretty UI. The discipline around it is
what separates pipelines from fires.
