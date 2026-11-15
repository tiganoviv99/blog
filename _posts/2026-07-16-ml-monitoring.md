---
layout: post
title: "Monitoring ML in production without drowning in alerts"
date: 2026-07-16
tags: [monitoring, mlops]
---

At some point a team starts monitoring everything: input drift, output
distributions, latency percentiles, feature null rates, model versions. Then
the alerts start, and within a month nobody reads them anymore.

Monitoring ML systems is not about collecting signals. It is about deciding,
in advance, what you will do about each one.

## Start from the business metric

The only signal that matters unconditionally is the business metric the model
exists to move. If it is healthy, most technical alarms are curiosities. If
it is sick, it tells you where to look. Everything else is diagnostic detail
to consult after the business metric moves, not before.

We put the business metric on a Grafana dashboard at the top, with a weekly
review ritual attached to it. That single habit caught more real problems
than every automated check we ever wrote.

## Separate drift you act on from drift you watch

Input drift alerts fire all the time, and most of the time nothing is wrong:
the catalogue changed, a campaign started, the season turned. The useful
question is not "did the distribution shift" but "does it matter". We kept
drift detection, but only alerted when drift coincided with a drop in
business metrics or in model confidence. Otherwise it stayed on the
dashboard, watched, not screamed.

## Alerts need owners and runbooks

An alert without an owner is noise with a schedule. Every alert we kept had a
named owner and a short runbook: what it means, what to check first, who to
page. Writing the runbook was also the test of whether the alert deserved to
exist: if the runbook was empty, the alert was deleted.

## Monitor the inputs, not just the outputs

By the time output metrics degrade, the damage is already live. Feature-level
checks on input data, freshness checks on upstream tables, and schema
validation caught problems before users could feel them. Freshness checks
alone saved us more than once, because a stale feature table produces
plausible-looking outputs for hours.

## Keep the alert count small on purpose

We treated alerts like a budget: a new alert had to displace an existing one.
This sounds bureaucratic and it is exactly the point. A small set of
high-quality alerts stays trusted. A wall of red becomes background noise,
and background noise is how real incidents get missed.

## The weekly review is the real monitor

Automation handles the surprises; humans handle the trends. Our most
valuable monitoring tool was a thirty-minute weekly review of the dashboard:
business metric first, then drift charts, then model versions and
experiments. Thirty minutes, every week, same time, with the decisions
written down.

That ritual caught slow decays that no alert could ever catch, because they
happened over weeks: a metric drifting down one percent a month, a feature
distribution stretching quietly. Alerts are great at fast and loud failures;
they are blind to slow and silent ones. The review is what sees those, and
the habit of writing decisions down is what makes the review actually lead
to action instead of nodding at charts.

The goal of ML monitoring is not total visibility. It is knowing, with
confidence, that when something actually breaks, someone will notice.
