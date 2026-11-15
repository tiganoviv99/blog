---
layout: post
title: "Tagging 330k products with an LLM, one batch at a time"
date: 2026-11-04
tags: [llm, nlp]
---

We needed better product tags. The catalogue had hundreds of thousands of
items, and the existing taxonomy was shallow, inconsistent, and quietly
hurting every downstream model that used it as a feature. Hand-labeling was
out of the question. So we built an incremental LLM-based annotation service
that enriched more than 330,000 products.

The interesting part was never the model. It was everything around it.

## Structured output is a contract, not a hope

The first version asked the model to "return tags as a list". It returned
lists, then prose, then lists again, then a list inside a sentence. Prompting
it to be consistent helps only so much. What actually fixed it was a schema:
we demanded strict JSON with a fixed set of keys, validated every response,
and threw invalid ones away automatically. The retry logic handled the rest.

Treat the model's output the way you treat any API boundary: validate, retry,
log the failures.

## Batch, but make batches small

330k items is not one job; it is hundreds of small jobs. We chunked the
catalogue into batches, ran them incrementally, and kept every batch
idempotent. A single item could be re-processed at any time without touching
its neighbours. That design decision alone saved us from a week of
reconciliation pain the first time the provider had a bad day.

## Cost control lives in the prompt and the pipeline

Token cost is a feature of two things: how much you send and how much you
ask for. We sent only the fields the model actually needed to see, never the
full product record. We also cached: if an item had not changed, we did not
re-annotate it. Cheap wins, but they compound at catalogue scale.

## Humans stayed in the loop, but at the right altitude

We did not review 330k outputs. We sampled. A human looked at a few hundred
items per category batch, flagged systematic errors, and we converted the
patterns into prompt or rule fixes. This turned manual review from
"verify everything" into "find the next systematic mistake", which is a much
better use of a person.

## The payoff showed up downstream

The enriched tags became features in search and ranking models, and the
improvement in downstream quality was visible enough that the annotation
service became a permanent part of the pipeline, not a one-off project.

## What still goes wrong

Three failure modes keep showing up. First, schema drift: the provider
returns a slightly different JSON structure one day, and the validation layer
rejects everything until someone notices the queue. We added monitoring on
rejection rates for exactly this reason. Second, silent degradation: the
model gets worse at one tag type, and the error only becomes visible in the
sampled review. That is what the sampling is for, and it must run on a
schedule, not when someone remembers. Third, over-trusting the annotations:
a tag with 99% model agreement is still not ground truth, and downstream
models inherit its mistakes. The pipeline is a loop: annotate, sample,
correct, repeat.

The lesson I keep from this: LLM projects fail in the plumbing. The
structured contract, the idempotent batching, and the sampling review are
the product. The model is just the worker.
