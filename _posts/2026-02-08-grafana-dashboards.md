---
layout: post
title: "Grafana dashboards that people actually open"
date: 2026-02-08
tags: [grafana, monitoring]
---

Most Grafana dashboards are monuments to a moment of enthusiasm. Someone
builds a dashboard with thirty panels, presents it once, and nobody opens
it again. The panels are fine; the design is wrong.

Here is what separates the dashboards people open from the monuments.

## One screen, one question

A dashboard should answer one question per screen. "Is the business metric
healthy?" belongs on the first screen, alone, large. Everything else waits
behind it. When we rebuilt our dashboards around single questions, the
weekly review stopped being a scroll-fest and became a series of yes-or-no
answers.

## Put the worst case at the top

People open dashboards when something feels wrong. The first thing they
should see is the number that tells them whether they are right: the
business metric, its trend, and how far it is from the alert threshold.
Drill-downs live below, and only for those who need them.

## Log scales are for friends, not dashboards

Anything that requires a legend to read has failed. We banned
double axes, tiny legends, and rainbow colour schemes. Two colours,
clear labels, big text. The harder the dashboard is to read, the less
often it gets read, and the whole point is reading it.

## Annotations are the memory of the system

Every deploy, every campaign, every incident, written onto the charts as
annotations. Without them, a metric dip is a mystery that someone has to
investigate from scratch. With them, it is usually "ah, the deploy from
Tuesday". Annotations are cheap and pay for themselves weekly.

## Weekly review or nothing

The best dashboard in the world rots without a standing meeting to look
at it. Thirty minutes, same time every week, decisions written down. The
dashboard is the tool; the review is the habit; the habit is the point.

## The weekly ritual that keeps it alive

A dashboard without a meeting dies, and a meeting without a format
wanders. Ours was fixed: thirty minutes, every week, the same three
questions in the same order. Is the business metric where we expect
it? What changed this week, and does the dashboard explain it? What
needs a decision, and who owns it by Friday?

The format mattered because it converted the dashboard from a
decoration into an instrument. The decisions got written down, the
follow-ups got owners, and the dashboard got annotations from the
deploys and incidents, which made the next week's reading easier. The
ritual was the actual monitoring system; the charts were its eyes.

Build dashboards like you expect a tired on-call engineer to read them at
2 a.m. Because one day, that is exactly who will.
