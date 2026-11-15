---
layout: post
title: "Metrics for ML teams: what to show your manager"
date: 2025-10-16
tags: [career, metrics]

Most ML teams underreport, overreport, or report the wrong things. The
first problem is fixable with tools; the other two are fixable only with
a clear idea of what a manager actually needs to see. Here is the
reporting stack that worked for us.

## One business number, always

Everything starts from the metric the team exists to move: revenue,
conversion, saved hours. It goes first, it goes large, and it gets a
trend. If the business number is healthy, the rest of the report is
context. If it is not, the rest of the report is the investigation.
Managers should never have to hunt for it.

## Model health in three lines

For each production model: version, last retrained, and the evaluation
number versus the previous version. Three lines per model, no more. The
point is to make staleness and regressions visible at a glance, not to
build a second dashboard in the report.

## The experiment funnel

How many experiments started, how many finished, how many shipped, and
what the shipped ones moved. This single table answers the question
managers actually have: is the team learning and converting learning
into value? A team with many starts and no ships has a process problem,
and the funnel makes it impossible to hide.

## Incidents and their cost

A one-line list of production incidents with duration and root cause
category. Over months, this accumulates into the most honest argument
for investing in reliability, because it is data rather than opinion.

## What not to report

Not every Jupyter exploration, not every model trained, not every metric
that moved in a notebook. The report is for decisions, not for
impression. Noise in the report trains the reader to skim, and a skimmed
report is a failed report.

## The frequency question

How often should the report go out? The honest answer depends on the
reader: a weekly version for the immediate manager, a monthly one for
the wider organisation. The weekly report is operational, almost
telemetry; the monthly report is narrative, with context and decisions.
Trying to serve both readers with one document produces a document
that serves neither.

We also learned the single most important trick: the report must be
readable without opening anything else. No links to dashboards as the
content; links as references only. If a sentence requires opening a
dashboard to be understood, it fails at its job. The report is the
dashboards' ambassador, not their table of contents.

The goal is simple: a manager reading for five minutes should know
whether the team is healthy and where the problems are. Build the report
for that reader, and it will be read.

One last observation: the best reports I have seen were written by
engineers who stopped treating reporting as overhead and started
treating it as the team's public interface. When the report is good,
the team gets resources, autonomy, and trust. When it is absent, the
team gets questions, audits, and second-guessing. Reporting is not the
tax on your work; it is how your work becomes visible enough to
survive.
