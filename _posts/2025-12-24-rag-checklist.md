---
layout: post
title: "RAG checklist: ten things to get right before the vector database"
date: 2025-12-24
tags: [rag, llm]
---

Teams building retrieval-augmented generation usually start by choosing a
vector database. That is like choosing paint before deciding what the room
is for. The vector database is the last decision, not the first.

Here is the checklist we worked through before touching infrastructure.

## One: define what "answered" means

A RAG system needs a definition of success per question type: fact lookup,
how-to, policy question. Without it, you cannot evaluate, and without
evaluation, you are just vibing with embeddings.

## Two: audit the source material

If the documents are outdated, contradictory, or half machine-generated,
the best retrieval in the world produces confident nonsense. We spent a
month cleaning sources before building, and it was the highest-return
month of the project.

## Three: decide the chunking policy

Chunk size and overlap are product decisions: what is the natural unit of
an answer in this domain? We chose chunks that map to real answer units
and kept metadata with every chunk, because metadata is what filters save
you later.

## Four: route before retrieving

Different questions need different sources. A lightweight router ahead of
retrieval was the cheapest accuracy win we found, far larger than any
embedding swap.

## Five: keep a lexical path

Embeddings alone fail on identifiers, names, and rare terms. A hybrid
setup with keyword search caught exactly the cases the vectors missed.

## Six: budget the context

More retrieved documents is not better. We measured answer quality against
the number of chunks and found the sweet spot, then treated it as a
budget.

## Seven: make the model cite its sources

Every claim traced to a retrieved chunk, or the model said it did not
know. This single rule changed user trust more than any model upgrade.

## Eight: evaluate refusal cases

The test set included questions the system should refuse. A model that
answers everything is a model that lies sometimes.

## Nine: log the failures

Every bad answer went into a review file with the route, the chunks, and
the prompt version. Reading fifty of those weekly drove the roadmap.

## Ten: version the prompts

Prompt changes are model changes. Versioned, evaluated, deployed with the
code.

## Deploy in this order

When we finally built it, the deployment order surprised nobody and
mattered enormously: retrieval and routing first, with the answerer
hardcoded where possible; then the model, with citations required from
day one; then the refusal logic; then the fancy parts, like
multi-step reasoning, only where a logged failure demanded them. Each
stage shipped to users earlier than the team expected, and each stage
paid for the next one with real feedback.

The order is the strategy. Infrastructure first means you learn whether
retrieval is even adequate before burning tokens on generation. If
retrieval is broken, no amount of model quality fixes it, and the
opposite order hides that fact until the launch review.

None of this required a vector database decision. Do the list first; the
database choice will feel obvious afterwards.
