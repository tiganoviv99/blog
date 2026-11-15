---
layout: post
title: "Agents that edit code: scary, useful, and inevitable"
date: 2025-09-08
tags: [agents, llm]

The idea of an agent writing code in a real repository used to be a
demo trick. Now it is a tool, and like every tool, it is exactly as good
as the constraints around it. Here is what changed when code-editing
agents entered our workflow.

## The scary part is not the code; it's the confidence

An agent writes code the way a very fast junior does: sometimes right,
sometimes plausible, always confident. The danger is not one bad diff.
It is the accumulation of plausible diffs that pass review because the
reviewer is tired and the diff is small and it almost looks right.

## The fix is the same as for juniors

Review everything, for a long time, until the agent earns trust in a
narrow area. We started with well-specified, low-risk tasks: tests,
refactors, small utilities. The agent got good at those, and only those
were left partially unsupervised. Scope discipline was the entire
safety model.

## Tests become the contract

For an agent, a good test suite is not a nice-to-have; it is the
specification. We wrote the tests first or asked the agent to write the
tests first, and judged the work by the tests. The agent's code improved
in proportion to the quality of the tests, which taught us something
about our own code too.

## The review burden is real

Agents produce code faster than humans can review it well, and the
bottleneck moves to the reviewer. We learned to review differently:
patterns over line-by-line, diffs in small batches, and explicit lists
of what the agent was not allowed to touch. Review capacity, not
generation capacity, became the constraint, and we planned around it.

## What it is good for, today

Boilerplate, migrations, tests, refactors with clear invariants,
tiresome small changes. What it is not good for: architecture, taste,
and anything where the spec is the hard part. That boundary moves every
quarter, but it always exists.

## The tooling layer matters more than the model

Most of the practical value of code agents lived in the surrounding
tooling, not in the model quality: how the agent sees diffs, how
easily it can run the tests, how well its context is scoped to the
task. An average model with excellent context beats an excellent model
with broken context, every time. We spent our effort accordingly:
clean task descriptions, tight file scopes, fast test feedback.

The teams that treat the agent as a raw text generator get raw text
back. The teams that build a small harness around it get a colleague.
The harness is not a nice extra; it is the product.

The agent did not replace the engineer. It replaced the least
interesting third of the engineer's week, and the teams that used it
that way were the ones that benefited. The teams that treated it as an
oracle got burned, and the burns were educational for everyone nearby.
