---
layout: post
title: "Onboarding a junior ML engineer: the first ninety days"
date: 2025-12-08
tags: [career, mentoring]
---

A junior ML engineer joins your team with solid fundamentals and no idea
how your production system works. The next ninety days decide whether
they become productive or quietly drown. Here is the onboarding plan that
worked for us, written from the mentor's side.

## Week one: the business before the models

Start with users, metrics, and money: what the product does, which
numbers matter, and how the team's work connects to them. A junior who
understands the business can be useful everywhere. One who only
understands models is useful nowhere, yet.

## The first task is a small, real, ugly one

Not a toy project: a real, small task in the actual pipeline. A logging
improvement, a monitoring check, a feature with a clear definition. Small
enough to finish, real enough to matter. The point is the complete loop:
code, review, deploy, verify, with all the boring ceremony that makes
production work.

## Pair on the debugging, not the coding

The highest-value time is debugging together: a metric that looks wrong, a
pipeline that failed, a model that misbehaves. Juniors learn the system's
anatomy fastest when watching an experienced engineer chase a real
problem. Schedule it; do not leave it to chance.

## Give them an evaluation to run

Nothing teaches the discipline faster than running an evaluation and
explaining why the number moved. It forces contact with the data, the
metrics, and the code all at once, and it produces a written artifact the
junior can own.

## Review in public, praise in private, and vice versa

Mistakes reviewed kindly in public teach the whole team. Praise in
private builds the individual. Both matter; the order matters too: never
let a junior discover they broke something from a stranger.

## What not to do

Three failure modes recurred, and all three are avoidable. Do not give
a junior a task without a written definition of done; vague tasks teach
anxiety, not engineering. Do not let the first month pass without at
least one production deployment, however small; nothing else builds the
muscle of shipping. And do not answer every question with a link to the
docs; the first week of questions is your best signal about what the
docs are missing, and fixing them is the fastest onboarding investment
you will ever make.

Ninety days later

The goal is not mastery. It is ownership of one small corner and the
confidence to ask. Everything else compounds from there. A well-onboarded
junior is the cheapest productivity win a team can make, and a badly
onboarded one is the most expensive loss.
