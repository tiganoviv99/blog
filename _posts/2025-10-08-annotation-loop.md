---
layout: post
title: "Human-in-the-loop annotation without burning out your annotators"
date: 2025-10-08
tags: [data, nlp]

Human annotation is the quiet engine of every model I have shipped, and
the quiet engine has a tendency to overheat. Annotators get tired, drift,
and quietly stop caring, and the quality of your data follows them down.
Here is what kept our annotation loops healthy.

## Annotate decisions, not entire documents

Asking a person to review a whole page produces skimming. Asking them to
judge one specific, framed decision produces consistency. We broke the
work into small units with one question each. Smaller units, sharper
answers, less fatigue.

## The guidelines are a living document

Vague guidelines produce private interpretations. We wrote the rules
down, kept examples current, and treated guideline confusion as a bug
in the guidelines, not in the annotator. Every ambiguous case that
surfaced became a new example in the document.

## Track agreement, not just throughput

Speed is easy to measure and dangerous to optimise. We tracked
inter-annotator agreement on a shared calibration set, and a falling
agreement number was the early warning that someone was tired or the
guidelines had gone stale. The calibration set was small, fixed, and
worth its weight in gold.

## Rotate the boring parts

Long stretches of the same task degrade attention. We rotated tasks,
varied difficulty, and capped sessions. The limits felt bureaucratic
until we compared the error rates of hour one and hour four. Then they
felt obvious.

## Close the loop back to the annotators

People annotate better when they see what their work becomes. We showed
annotators the model's mistakes that their labels caught, and the
accuracy numbers their batches produced. Feedback turned a tedious job
into a meaningful one, and meaning is the best defence against burnout.

## What breaks first

Every annotation loop we ran degraded in the same order. First the
guidelines went stale: the product changed, the examples stopped
matching reality, and annotators quietly improvised. Then agreement
dropped, a few points at a time, invisible unless someone was watching.
Then a category collapsed entirely, and the model trained on the
broken labels regressed in production a month later.

The whole chain takes weeks to unfold and minutes to prevent: review
the guidelines monthly, watch the agreement chart weekly, and treat a
falling line as an incident, not a footnote. The annotators are not
the weak link in this chain. The maintenance schedule is.

Annotation quality is not a procurement problem. It is a design problem:
tasks, guidelines, calibration, rotation, feedback. Design it well, and
the engine keeps running.

The teams that treat annotators as a plug-in service get plug-in
results: adequate at first, silently degrading later, expensive to
rebuild. The teams that treat annotation as part of the product build
a capability that compounds, because every well-judged label is a
small deposit into every model the team will ever train.
