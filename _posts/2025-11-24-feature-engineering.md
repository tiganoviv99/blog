---
layout: post
title: "Feature engineering is debugging in slow motion"
date: 2025-11-24
tags: [features, data]
---

There is a moment in every ML project when someone says "the model is
stuck, let's add features". Adding features is fine. But the phrase hides
the truth: feature engineering is mostly discovering, through slow and
painful iteration, that your data was wrong in ways you had not imagined.

## Every feature encodes an assumption

"Price in the last seven days" assumes seven days is the right window and
that price is what matters. "Items viewed this session" assumes views
carry intent. Every feature is a claim about the world, and most of the
claims are wrong in some segment. The work is finding which claims hold.

## The nulls are the story

A feature that is null for 40% of users is not a feature; it is a report
about a broken upstream table. We learned to plot the null rate of every
feature before believing any of them. Several "weak" features became
strong the moment we fixed the nulls.

## Watch the distributions, not the aggregates

Averages lie. A feature whose mean is stable can still have wild tails,
and the tails are where the model learns nonsense. Histograms, quantiles,
and a few hand-picked rows told us more in an hour than summary statistics
told us in a week.

## Features age like fish

A feature that works this quarter rots next quarter: seasonality shifts,
definitions drift, sources change hands. Every feature needs an owner and
a refresh check. The features we tracked as assets survived; the ones we
treated as one-off experiments became landmines.

## Feature validation habits

Before trusting any new feature, we ran the same three checks. First,
the null audit: what does missing look like, and is it actually
missing, or is it a broken upstream? Second, the distribution plot
over time: stable features show stable shapes, and anything jagged
deserves a story. Third, the leakage sniff test: would this feature
have been available at prediction time in production? A feature that
fails that test will make your offline numbers sing and your production
numbers cry.

These checks are mechanical, quick, and unglamorous. They also caught
more bad features than every ablation study combined. The lesson is
embarrassing in its simplicity: before asking whether a feature helps
the model, ask whether the feature is even what you think it is.

The honest framing

Feature engineering feels like craft, and it is: slow, empirical,
incremental. The model is the final paragraph of a long argument with the
data. If the argument was sloppy, the paragraph will not save it. Spend
the time where the truth actually lives.
