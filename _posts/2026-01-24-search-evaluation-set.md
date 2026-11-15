---
layout: post
title: "How we built a search evaluation set that people didn't hate"
date: 2026-01-24
tags: [evaluation, search]
---

Everyone agrees search needs an evaluation set. Almost nobody builds one,
because evaluation sets for search are famously tedious and controversial.
We built one anyway, and after some false starts, we found a process that
the team actually kept using.

## Start from real, recent queries

The first mistake is dreaming up queries. Dreamed queries are easy and
clean, and they measure nothing real. We pulled queries from production
logs: the long tail, the misspellings, the strange phrasings. The set was
messy on purpose, because production is messy.

## Judge relevance, not beauty

The second mistake is grading results by how good they look. We graded
against a written standard: does this result answer the query, and how
strongly? Four levels, explicit examples for each level, and the standard
published next to the judging tool. Disagreements went to a second judge,
and the pattern of disagreements improved the standard.

## Keep it small enough to judge by hand

A thousand queries judged once is a monument. Two hundred queries judged
monthly is a living set. We kept it small, refreshed a slice of it every
month, and retired queries that stopped appearing in production. A small
living set beats a large dead one every time.

## Make the metric boring and public

One number, computed the same way, shown in the same place, with the
history of every model change next to it. When the metric is boring and
public, it stops being a weapon in arguments and becomes infrastructure.
That is exactly what you want from an evaluation set.

## What broke it

Two things killed evaluation sets in the past, and both are boring.
First, neglect: the set stayed frozen while production moved, so
queries people stopped typing kept judging models nobody shipped.
Second, politics: someone "improved" a judgment to make their model
look better, and the set quietly stopped being trusted. Both failures
had the same cure: the set had an owner, a public changelog, and a
rule that no one edits judgments without a second person.

An evaluation set is infrastructure, and infrastructure needs the same
care as any service: ownership, change control, and maintenance. When
we treated it that way, it survived. When we did not, it rotted.

The payoff

The set killed more bad ideas than any review meeting. "Your model scores
worse on the set" is a conversation ender in the best sense. Building it
took a month of tedious work. Keeping it alive takes an hour a week. It
has been the best investment-per-hour in the whole search project.
