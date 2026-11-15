---
layout: post
title: "Evaluation sets you can trust, or why your metrics lie"
date: 2026-08-20
tags: [evaluation, nlp]
---

Every ML team I have worked on eventually had the same moment: an evaluation
metric says the model improved, everyone is happy, and then production says
otherwise. The metric was not lying out of malice. It was lying because the
evaluation set was answering a different question than the product asked.

Building an evaluation set you can trust is slow, unglamorous work. It is
also the highest-leverage work in ML.

## Leakage is the default state

If your test set shares authors, time periods, or users with your training
set, your numbers are optimistic. Temporal splits matter more than people
think: train on the past, test on the future, and watch the metric drop. That
drop is not a bug. It is the truth reasserting itself.

## Label quality beats label quantity

A thousand labels where half are wrong will reliably steer you toward the
wrong model. We spent time defining what a "good" answer meant, wrote the
guidelines down, and sampled judgments for consistency. Cleaning fifty
clearly wrong labels often moved the leaderboard more than a week of
architecture experiments.

## One number is not an evaluation

Accuracy alone hides everything interesting. We evaluated the slices that
mattered: rare queries, new users, long-tail categories. A model can win
overall while regressing exactly the segment the business cares about. The
slices are where the real product decisions live.

## Refresh or rot

An evaluation set is not a monument; it is a garden. Production data drifts,
query patterns change, and the set's hardest cases become easy while new hard
cases appear. We refreshed ours on a cadence and added cases from real
failures, so the set kept measuring what the product actually does.

## The metric is a filter, not a verdict

Even a great offline number is only a gate. We treated evaluation as a
filter: good enough to try, or not. The verdict always came from an A/B test
on real users. Offline metrics tell you what is worth testing; they do not
tell you what will work.

## Human evaluation is part of the system

For the cases that matter most, automated judgments are not enough. We ran
periodic human evaluation on a sample of model outputs, with written
guidelines and a second annotator on disagreements. It was slow and it
revealed what the automated metrics could not: nuance, tone, and the
"correct but useless" answer category that metrics love and users hate.

The inter-annotator agreement number itself became a quality signal for the
guidelines. When agreement dropped, it usually meant the guidelines had gone
stale, not that the annotators got worse. Keeping that loop healthy was
cheaper than any model change and did more for real quality. Automation
scales judgment; it does not replace it.

The uncomfortable truth is that a trustworthy evaluation set is a product in
its own right, with owners, guidelines, and maintenance. Teams that treat it
as a spreadsheet to fill once pay for it in bad launches. The teams that
invest in it make better decisions with worse models than everyone else
makes with better ones.
