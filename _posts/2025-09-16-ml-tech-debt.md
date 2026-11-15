---
layout: post
title: "Everything I know about technical debt in ML systems"
date: 2025-09-16
tags: [mlops, tech-debt]

ML systems accrue debt faster than almost any other software, because
they have all the usual debt plus the special kind that hides inside
data, models, and the space between them. Here is everything I have
learned about it, compressed.

## ML debt is invisible until it is expensive

A slow function annoys you immediately. A subtly wrong feature, a
stale evaluation set, a model trained on a deprecated definition: these
are silent until they are expensive. ML debt compounds in the dark,
which is why it must be hunted, not merely noticed.

## The classic sources

Model code has a habit of growing glue around it: preprocessing in
three places, feature logic duplicated between training and serving,
evaluation scripts that only the original author can run. Each piece is
reasonable at creation and toxic in aggregate. The tax shows up as
slow releases and mysterious bugs.

## The data-shaped sources

Data debt is the worst kind: undocumented schemas, pipelines with no
owner, labels whose meaning drifted, snapshots that cannot be rebuilt.
Data debt cannot be refactored in a sprint. It has to be backfilled,
re-labelled, or lived with, and living with it quietly caps the quality
of every model downstream.

## The model-shaped sources

Every deployed model is a liability that must be monitored, retrained,
and eventually retired. Models nobody retires pile up like cars in a
yard. Each one serves traffic, consumes attention during incidents, and
costs a little credibility every time it misbehaves.

## The only strategy that worked

Debt is not evil; it is a loan, and loans can be rational. The trick is
budgeting repayment like a first-class activity: a fixed slice of every
sprint, a visible debt list, and a rule that every new model registers
its retirement plan at launch. The teams that treated debt as
infrastructure maintenance stayed fast. The teams that treated it as an
interruption slowed down in ways they never attributed to debt, which
is exactly what makes debt dangerous.

## How we made it visible

Debt only gets managed when it is measured. We kept a simple list:
every known debt item, an estimate of what it costs per month in
incidents, slowed releases, or maintenance time, and a one-line plan.
The list lived in the open, next to the roadmap, and new items were
added during retrospectives without shame. Visibility changed the
conversation from "debt is someone else's problem" to "here is the
queue, what do we retire this sprint".

The list also had an unexpected side effect: it made us reject new
debt early. When adding a shortcut required writing it on the board
with a cost, people thought twice. Not always. But twice is enough to
change a culture.

Debt is not the enemy. Invisible, unmanaged debt is. Make it visible,
make it scheduled, and it becomes just another engineering task.
