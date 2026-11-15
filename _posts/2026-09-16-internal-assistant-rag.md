---
layout: post
title: "An internal assistant over the codebase and wikis: route before you retrieve"
date: 2026-09-16
tags: [agents, rag, llm]
---

We built an internal assistant that answers questions across the codebase,
wikis, and assorted internal data sources. The most valuable decision we made
had nothing to do with model quality: we routed the question before we
retrieved anything.

## Routing is the cheapest accuracy win

"Where is the retry logic in the checkout service?" and "How do I onboard a
new team member?" need completely different retrieval backends. A single
pipeline that treats both the same way produces answers that are wrong in
interesting ways. We built a lightweight router that classified the intent
and dispatched the question to the right tool: code search, wiki search, or
direct answer.

The router itself was a simple model, but its effect was larger than every
embedding swap we tried afterwards.

## RAG is a pipeline, not a feature

Retrieval-augmented generation sounds like one thing, but it is a chain of
decisions: chunking strategy, which metadata survives the chunk, how many
documents to retrieve, how they are ordered, and what the final prompt is
allowed to say. Every link in that chain can silently degrade quality. We
treated each link as a component with its own evaluation, instead of
evaluating only the final answer.

## Ground the answer, and let the model say "I don't know"

Hallucinations in an internal assistant are not embarrassing; they are
dangerous, because people act on the answers. We forced every claim to cite
its source, and we allowed the model to refuse when retrieval came back
empty. A confident refusal is a feature. Users prefer "I could not find
that" over a plausible lie they discover in production.

## Multi-agent, but only where it earns its complexity

The system grew into a small multi-agent setup: a router, a retriever, and an
answerer, each with a narrow job. We resisted adding more agents. Every agent
is another failure mode, another prompt to maintain, another latency
contribution. The rule we used: a new agent must replace at least one
human step, not decorate an existing one.

## Evaluation is the product

We built an evaluation set of a few hundred real questions with judged
answers, including the "should not answer" cases. Every change ran against
it. The set was boring to maintain and it was the reason the assistant stayed
usable as the codebase moved underneath it.

## The questions nobody expected

The most useful thing we built after launch was a weekly review of the
questions the assistant could not answer. It was a list of user questions,
the route the router chose, what was retrieved, and where the answer broke
down. Reading fifty of those in an hour beat any dashboard.

The patterns were humbling. People asked about versions, about
interconnections, about why something was built a certain way. Our chunks
often had the code but not the decision behind it. That single insight
changed the indexing strategy more than any model upgrade. Log the failures,
read them regularly, and let them drive the roadmap.

The takeaway: for internal assistants, retrieval strategy beats model choice,
routing beats retrieval, and evaluation beats everything.
