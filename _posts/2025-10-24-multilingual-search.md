---
layout: post
title: "Multilingual search: when one model isn't enough"
date: 2025-10-24
tags: [search, nlp]
---

A search feature that works in one language is a local feature. The
moment the product goes multilingual, everything gets harder in ways
that feel subtle until they are not. Here is what we learned when
queries started arriving in more than one language.

## The long tail of languages is the product

The first language is easy. The fifth is where the real work is: less
training data, noisy queries, borrowed scripts, regional variants. Users
in the long tail judge the product by the same standard as everyone else,
and the long tail is where multilingual search lives or dies.

## Translation is a feature, not a shortcut

Translating every query into one canonical language feels elegant and
breaks quietly: slang, names, and intent survive translation poorly. We
used translation as one signal among several, never as the only path.
Query understanding ran on the original text; translation helped only
when it genuinely clarified intent.

## Language detection is quietly critical

Routing a query to the wrong language stack produces errors that users
cannot explain, only feel. We built language detection early, logged its
failures, and treated misdetections as first-class bugs. A short
code-mixed query can defeat naive detectors, and those cases became a
permanent part of the evaluation set.

## Embeddings help, but the data decides

Multilingual embedding models sound like a free lunch: one vector space,
all languages. In practice, quality varied wildly by language pair, and
the difference came down to training data coverage. The model choice
mattered less than knowing which languages were actually supported and
testing them explicitly.

## Evaluate per language

A global metric hides a language where search is broken. We sliced the
evaluation by language and set a minimum bar per slice. The slices that
failed got resources before anyone noticed them in production, because
the metric made the problem visible while it was still cheap to fix.

## Infrastructure notes for the long tail

Language support is not a model property; it is a pipeline property.
Every language needs tokenisation that does not mangle its script,
normalisation rules that respect its conventions, and an evaluation
slice with enough queries to be meaningful. None of this is glamorous,
and all of it is where long-tail languages actually fail.

We also kept a simple rule: no language ships without a named owner
and a minimum evaluation bar. Languages without owners drifted into
brokenness silently, because nobody felt them breaking. The ownership
rule felt bureaucratic and turned out to be the single most reliable
predictor of language quality in the product.

Multilingual search is not one search system with extra languages. It is
many small search problems sharing infrastructure, and the teams that
treat it that way are the ones whose users stay.

There is a quiet principle underneath all of this: language quality is
a commitment, not a feature toggle. Every language you support is a
promise you will keep answering questions about, and the promise is
made to the users who have the fewest alternatives. Keep the promise
deliberately, or do not make it. Half-supported languages are worse
than none, because they teach users to expect disappointment.
