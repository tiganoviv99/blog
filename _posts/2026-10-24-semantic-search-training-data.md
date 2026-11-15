---
layout: post
title: "Semantic search without good training data is just expensive matching"
date: 2026-10-24
tags: [search, embeddings, nlp]
---

I worked on a semantic search project where the first thing we learned was
embarrassing: swapping the embedding model changed almost nothing. Improving
the training data changed everything.

Most conversations about semantic search start with model choice and end with
model choice. In practice, that is the smallest lever.

## The model is a commodity

Embedding models are interchangeable to a degree that surprises people. We ran
A/B tests where the only change was the encoder, and product metrics barely
moved. What moved them were the queries we could now match that we could not
match before, and the noise we stopped letting through. Both of those are
data problems, not model problems.

## The real asset is the query-to-result feedback

Clicks, purchases, and dwell time are the ground truth of search, but they
arrive dirty: position bias, intent ambiguity, and cold queries with no
history at all. The core of the project became building a training set that
the models could actually learn from, with careful negative sampling and
temporal splits that prevented leakage. This part has no shortcut, and no
blog post can replace it.

## Hybrid beats pure on both ends

Pure vector search is wonderful for fuzzy intent and terrible for exact
matches, identifiers, and rare items. Pure lexical search is the opposite.
The production system used both and merged the results. The boring BM25 side
kept the head queries honest; the vector side carried the long tail. If your
architecture has only one retrieval path, the first experiment to run is
adding the second.

## Evaluate before you optimize

We maintained an evaluation set of real queries with relevance judgments,
updated regularly, with explicit rules about what a "good" result meant for
the business. Every model change went through it before any A/B test. It was
slow to build and slower to maintain, and it paid for itself by killing bad
ideas before they reached users.

## The A/B tests that "failed"

Several promising variants lost in A/B tests. Those losses were the most
useful results of the whole project, because they forced us to look at what
users actually did rather than what our embeddings thought was similar.
Similarity is not relevance, and relevance is not usefulness.

## When semantic search is the wrong answer

It is worth saying out loud: not every search problem is a semantic search
problem. If your users type exact identifiers, categories, or codes, a
well-tuned lexical search with synonyms will beat a vector system at a tenth
of the cost. We had a segment like that, and we deliberately kept it on the
lexical path instead of forcing everything through embeddings.

The honest framing is that semantic search expands the queries you can
handle; it does not replace the ones you already handle well. Build it for
the long tail, keep the head on the boring path, and let the evaluation set
tell you where the boundary is.

If you are starting a semantic search effort, spend your first month on the
feedback data, the evaluation set, and the hybrid pipeline. Pick an embedding
model you can replace in an afternoon. You will probably have to.
