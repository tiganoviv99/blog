---
layout: post
title: "From 82% to 94% recognition accuracy: the data pipeline was the model"
date: 2026-09-12
tags: [cv, data]
---

A self-checkout system needed to recognise products visually. The recognition
model started at 82% accuracy and finished at 94%. Ask anyone to guess what
changed and they will say "better architecture". They would be wrong.

The biggest lever was the dataset, and specifically an automated
generation pipeline for it.

## Labeling was the bottleneck, so we industrialised it

Manual labeling does not scale, and it degrades: after a few thousand images,
even careful annotators drift. We built a pipeline that handled collection,
labeling, and augmentation end to end. Some of the data was collected by the
system itself during real operation, which gave us exactly the distribution
the model would face rather than the distribution we imagined it would face.

The training set grew roughly fivefold, and manual labeling effort dropped by
around 60%. That trade alone justified the pipeline before any accuracy gain.

## Augmentation is a domain skill

Generic augmentation helps a little. Domain-aware augmentation helps a lot.
Products at a checkout appear under store lighting, partial occlusion, and
odd angles. Augmenting with exactly those distortions, rather than random
rotations of everything, was the difference between a model that memorised
clean studio shots and one that worked at 9 p.m. under fluorescent light.

## The architecture barely changed

This is the part that still surprises people. Between the 82% version and the
94% version, the core architecture moved only modestly. The wins came from
data volume, data realism, and cleaning the label noise. In computer vision,
a mediocre model on great data routinely beats a great model on mediocre
data.

## Cleaning is a loop, not a step

The first version of the pipeline produced data with systematic errors: some
categories overrepresented, some lighting conditions almost absent. We added
analytics over the dataset itself: class balance, brightness histograms,
failure slices from the model's own confusion matrix. The pipeline improved
because we watched where the model failed and fed those cases back in.

## The general lesson

## The feedback loop with the model

An underrated part of the pipeline was closing the loop from model failures
back to data. Every week we took the confusion matrix, picked the categories
where the model was weakest, and generated targeted training cases for
exactly those situations.

This is a different mindset from "collect more data". More of the same
distribution gives diminishing returns. Targeted data for known failure
slices gives compounding ones. The accuracy improvements were not smooth;
they came in steps, each one after a deliberate batch of hard cases went
into the training set.

The pipeline was not a one-time migration from manual to automatic. It was a
machine for converting model confusion into training signal, run on a
schedule. That machine, more than any architecture, is what carried the
model from 82% to 94%.

We like to believe progress is a story of better algorithms. In my
experience, most production CV wins are a story of better data logistics:
capture, clean, balance, augment, repeat. If your CV project is stuck, stop
tuning the model and start auditing the data. It is less glamorous, and it
works.
