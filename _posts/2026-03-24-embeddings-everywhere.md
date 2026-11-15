---
layout: post
title: "Embeddings everywhere: the quiet cost of vectorising everything"
date: 2026-03-24
tags: [embeddings, search]
---

At some point, embeddings became the default answer to every data problem.
Similar products? Embeddings. Duplicate detection? Embeddings. Clustering,
classification, the office intranet search? Embeddings.

They work. They also have costs that nobody budgets for, and the phrase
"let's just embed it" hides most of them.

## Indexing is where the money goes

Embedding the data is the cheap part. Storing it, indexing it, and keeping
the index fresh is where the real cost lives. Vector indexes are not
free: memory, latency, and a whole new failure mode every time the index
drifts from the source data. Before embedding anything, ask who owns the
index.

## Vectors are a lossy summary

An embedding keeps the meaning and drops the details. For fuzzy similarity
that is the point. For exact matching, identifiers, codes, and precise
filters, it is a bug in disguise. We learned to combine: vectors for the
fuzzy layer, structured filters for the exact layer, and never one without
the other.

## The refresh problem never ends

Data changes: products appear, documents update, users churn. An embedding
pipeline that runs once is a snapshot that rots. The durable solution was a
scheduled re-embedding pipeline with clear freshness guarantees, which is
exactly the boring operational work the word "just embed it" was supposed
to avoid.

## Semantic similarity is not the same as your problem

Two sentences can be semantically close and practically useless for each
other. The embedding measures one thing, and your product needs another.
The gap between them is paid in bad recommendations and confused users,
and it only shows up in evaluation, never in the embedding itself.

## When not to embed

Exact identifiers, small vocabularies, well-defined taxonomies: a lookup
table or a rule beats a vector system on cost, latency, and
maintainability. We kept a list of "not embedding this" cases, and it was
as valuable as the list of things we did embed.

## The hybrid pattern that kept us sane

The durable architecture was hybrid in a boring, predictable way: a
lexical path for exactness, a vector path for fuzziness, and a merge
step that treated them as two opinions rather than one pipeline. The
merge logic itself became a small, testable component with its own
evaluation, because that is where the pathologies appeared: vector
results drowning lexical hits, or the reverse.

Hybrid systems are more parts to maintain, and the temptation is to
avoid them. Resist the temptation. The failure modes of each path are
different and mostly non-overlapping, so the combination fails less
often than either alone. In production, "less often" is the entire
point.

Embeddings are a tool, not a worldview. Use them where fuzziness is the
product, and keep the boring exact layer alive next to them.
