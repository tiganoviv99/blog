---
layout: post
title: "False positives at self-checkout: optimise the error that costs money"
date: 2026-09-08
tags: [cv, metrics]
---

In a self-checkout system, the headline metric is recognition accuracy. But
not all errors are equal, and pretending they are leads you to optimise the
wrong thing.

Our model improved its robustness to occlusion and lighting variation and cut
false-positive detections by roughly 30%. The story behind that number is a
story about choosing what to optimise.

## False positives and false negatives have different prices

A false negative is a product the system fails to recognise: the user picks
it up again, and the machine asks them to rescan. Annoying, visible, but
cheap. A false positive is a product the system recognises wrongly: the wrong
item lands on the receipt, and the customer is charged for something they did
not take.

One of these errors generates friction. The other generates money movement,
disputes, and trust damage. They are not symmetric, and a single accuracy
number hides exactly the difference that matters.

## Thresholds are a business tool

The model produces probabilities; the business decides the threshold. Where
we set that threshold was a product decision disguised as a technical one. We
made it explicit: for high-cost items and weight-mismatched detections we
required much higher confidence, effectively accepting more false negatives
in exchange for almost no false positives.

This kind of asymmetric thresholding is common in fraud, medicine, and
payments, and it applies perfectly to checkout. A flat 0.5 cutoff for every
category was leaving money and trust on the table.

## Robustness to occlusion and lighting was the real fix

The threshold tuning worked, but the durable improvement came from training
the model on the conditions it actually faced: items partially covered by
other items, reflections, tired evening lighting. We systematically added
those cases to the training data and to the evaluation set. A model that has
seen occlusion in training stops hallucinating detections in production.

## Evaluate per error type, not one blended number

We split the evaluation into false-positive rate, false-negative rate, and a
few cost-weighted slices. Every model change had to state its effect on each.
This simple discipline ended the era of "accuracy went up, and something
mysteriously got worse" discussions, because the breakdown showed exactly
what got worse.

## Calibration matters more than it looks

When the model says 0.9, what does that mean? If 0.9 means "correct nine
times out of ten", thresholds are honest tools. If 0.9 means whatever the
model felt that day, every threshold becomes guesswork. We looked at
calibration per category, because a confident wrong detection on a cheap
item is very different from a confident wrong detection on an expensive one.

Calibration also changed how we communicated with the product side. Instead
of arguing about abstract confidence cutoffs, we could talk in the language
they already used: how many wrong charges per thousand transactions, and how
many rescan prompts. When the numbers mean the same thing to both sides,
decisions get easier.

The lesson generalises well beyond computer vision: accuracy is a summary,
and summaries hide the costs. Find the error that costs money, name it,
measure it separately, and optimise that.
