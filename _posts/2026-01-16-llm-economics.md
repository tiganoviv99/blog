---
layout: post
title: "The economics of an LLM feature: latency, cost and value"
date: 2026-01-16
tags: [llm, economics]
---

A demo LLM feature and a production LLM feature are separated by one
question that demos never answer: what does it cost per request, and what
is a request worth?

I have seen more LLM projects die at that question than at any technical
problem.

## Latency is part of the product

Users do not experience your cleverness; they experience the wait. A
brilliant answer in eight seconds is worse than a good answer in one.
Before building anything, we set a latency budget and designed the
pipeline to it: smaller models for the fast path, caching, streaming
partial results. Latency was a first-class requirement, not a surprise
discovered at launch.

## Cost per request is a design variable

Token cost, model size, request rate: these multiply fast at production
scale. We computed cost per thousand requests before choosing a model, and
the answer routinely ruled out the most impressive option. The right
question is rarely "which model is best" and usually "which model is good
enough for this task at this price".

## Value is the part nobody measures

A feature that saves an employee ten minutes a day has a value, and it
can be estimated. A feature that reduces support tickets has a value too.
We wrote the expected value next to the expected cost before starting,
even with rough numbers. The rough numbers were enough to kill the bad
ideas and greenlight the good ones.

## Caching and tiering are your friends

Most requests do not need the frontier model. We routed by difficulty:
simple requests to a cheap fast path, hard ones to the expensive model,
repeated ones to the cache. Tiering converted several features from
economically impossible to comfortably profitable without losing quality
where it mattered.

## A worked example

Suppose a support-summary feature handles a thousand requests a day,
saves a human two minutes each, and each saved minute is worth roughly
fifty cents. That is a thousand dollars a day of value, or about
thirty thousand a month. Now the cost side: at two cents per request
all-in, the feature costs twenty dollars a day. The economics are
comfortable, and the project is worth building.

Change one assumption and the verdict flips. If the model is ten times
more expensive and saves only thirty seconds, the feature loses money.
The value of the exercise is not precision; it is that the assumptions
are now visible and debatable. Teams that run the numbers this way kill
bad projects in an afternoon and defend good ones with arithmetic
instead of adjectives.

The discipline

For every LLM feature, write down three numbers before the first line of
code: latency budget, cost per thousand requests, expected value per
request. If any of the three is missing, you are building a demo, not a
feature.
