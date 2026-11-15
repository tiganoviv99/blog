---
layout: post
title: "Drift is not the problem. Stale features are"
date: 2026-02-24
tags: [monitoring, features]
---

ML monitoring culture is obsessed with drift: input drift, output drift,
concept drift, drift dashboards. After years of watching those dashboards,
I can say it plainly: the drift alerts were mostly noise, and the real
incidents came from somewhere else entirely.

## The actual culprit: a stale feature

The recurring production incident was not a slowly shifting distribution.
It was a feature table that stopped updating. The model kept serving
predictions, the business metrics sagged, and the drift charts eventually
lit up, hours later and without naming the cause.

A freshness check on the upstream table would have caught it in minutes.

## Drift is often just a normal Tuesday

Distributions shift all the time: campaigns, seasons, new catalogue
entries. Most shifts are benign, and alerting on every one trains the team
to ignore the alerts. Drift detection is a diagnostic tool, not an alarm.
The alarm should fire on the thing that actually breaks: staleness,
schemas, and the business metric.

## Watch the pipeline, not just the model

The model is a small part of the system. The upstream jobs, the feature
store, the serving layer: that is where things actually fail. Our
monitoring shifted accordingly: freshness checks on every input table,
schema validation on every ingest, and a watchdog on the pipeline itself.

## The order of evidence that works

When something looks wrong, we learned to check in a fixed order: business
metric first, then upstream freshness, then serving health, then drift
last. Drift being last is not a demotion; it is the correct place for a
slow, noisy signal.

## The freshness checklist

If I were building monitoring from scratch today, the first deliverable
would be a freshness checklist, not a drift dashboard. For every input
table: how fresh should it be, how do we check it, who gets paged when
it goes stale, and what is the runbook. Twenty tables, twenty lines,
one afternoon of work, and it catches the failure mode that actually
happens.

The checklist also forces a useful conversation: which features can
tolerate a day of staleness and which cannot. That conversation is
really about the product, not the platform, and it is better had
before the incident than during it. Freshness checks are the
unfashionable sibling of drift detection, and they do the real work.

The honest reframe

Drift dashboards are popular because they are easy to build and look
impressive. Freshness checks are unpopular because they are boring and
require knowing where every feature comes from. And yet the boring checks
are the ones that would have caught our real incidents.

Build the boring thing first. Monitor what actually breaks, not what
feels sophisticated.
