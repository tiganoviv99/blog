---
layout: post
title: "The five-minute model review"
date: 2026-03-08
tags: [process, mlops]
---

Code has code review. Models deserve the same, but the standard ML process
replaces it with a long, ritualised meeting where nobody looks at anything
specific. I want to make the case for a lighter ritual: the five-minute
model review, run before every launch.

## It is not a lecture

The five-minute review has one goal: force the author to answer five
questions out loud, with evidence, in front of at least one other person.
It is short on purpose. If it takes longer, the model was not ready to
launch.

## The five questions

One: what metric are we trying to move, and what would count as a win?
Two: what could go wrong, and how will we know if it does? Three: what
does the evaluation set say, and where does it disagree with itself?
Four: what is the rollback, and has anyone actually run it? Five: who
owns this model tomorrow, not today?

## The real function is forcing articulation

Most of the value is not in the answers. It is in watching an engineer
discover, mid-sentence, that they cannot answer one of the questions. That
moment, in five minutes, is cheaper than the same discovery three weeks
into production.

## Evidence beats slides

The review is a document inspection, not a presentation. Open the
evaluation numbers, open the monitoring plan, open the rollback runbook.
Slides are what we use when the artifacts are weak, and weak artifacts
are exactly what the review exists to catch.

## It compounds

Five minutes per model, a couple of models a month: the cost is tiny and
the habit compounds. Teams that run it ship with fewer incidents and
decommission faster, because ownership and rollback get decided before
the launch, not after the fire.

## How to start

Do not announce a new process. Just run it. Take the next model that is
about to launch, sit down with the author, ask the five questions, and
write the answers in the launch doc. That is the entire onboarding:
one real review, on a real model, with the answers recorded.

The second review is easier, because the author has seen what the
first one caught. By the fifth, the team starts pre-answering the
questions in their launch docs without being asked, which is the point
where the ritual has become a habit. Processes this small do not need
buy-in; they need repetition. Give it a month, and the launch docs
will read like the review checklist, because they will be the review
checklist.

Steal the format. It is the cheapest quality process I have ever seen,
and the most durable.
