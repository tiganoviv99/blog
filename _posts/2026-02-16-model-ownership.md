---
layout: post
title: "Handoffs and the slow death of model ownership"
date: 2026-02-16
tags: [career, process]
---

Every organisation wants machine learning, and almost every organisation
underestimates one thing: models are not built and finished. They are
owned, fed, monitored, and eventually buried. When nobody is responsible
for that life cycle, the models quietly rot, and everyone is surprised.

## A model without an owner is an orphan

Orphaned models are everywhere: serving traffic nobody watches, trained
on data nobody updates, maintained by teams that no longer exist. The fix
is administrative, not technical: every model in production has exactly
one named owner, and "the team" is not a name.

## Handoffs are where ownership dies

When the person who built the model leaves or moves on, the knowledge
usually leaves with them. The handoff, if it happens at all, is a
forty-minute call. We started treating handoffs as deliverables: a
runbook, a monitoring checklist, a documented rollback, and a week of
co-owning before the old owner disappears.

## Bus factor is a metric

We tracked it bluntly: how many people would have to leave before a model
becomes unmaintainable? The answer was usually one, and the moment we
made that visible, the team started writing things down without being
asked.

## Rotation beats heroism

Sole owners who fix everything at midnight become single points of
failure. Rotating the on-call and the ownership across the team was
unpopular for a month and then saved everyone. Ownership does not mean
doing everything; it means being accountable for the model's health and
knowing where the runbooks are.

## Succession is a design feature

Ownership only survives if succession is designed, not improvised.
For every production model, we kept a named backup: someone who had
shadowed the owner, run the runbook, and could take over on a day's
notice. Rotating that backup role across the team meant that within a
year, every model had several people who understood it well enough to
hold it steady.

The exercise surfaced something unexpected: writing the runbook for a
backup is itself the best test of whether the owner actually
understands their own model. Half the runbooks we wrote exposed gaps
the owner patched on the spot. Succession planning was not just
insurance; it was a review process wearing a different hat.

The takeaway

Models fail socially before they fail technically. Ownership, handoffs,
and bus factors are not HR topics; they are reliability topics. If you
want your models to survive their creators, make ownership an explicit
part of the system, not an assumption.
