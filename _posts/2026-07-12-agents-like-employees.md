---
layout: post
title: "Agents: treat them like employees, not functions"
date: 2026-07-12
tags: [agents, llm]
---

The most useful mental model I have found for building LLM agents is also
the simplest one: treat the agent like a new employee.

A new employee gets a clear job description, tools, a budget, and a manager
who checks their work. An agent needs exactly the same things, and projects
go wrong in exactly the same ways when those things are missing.

## The job description is the prompt

A vague instruction like "answer questions about our product" is how you get
confident nonsense. A good agent prompt is a job description: what the agent
knows, what it may do, what it must never do, and what "done" looks like.
Writing it takes time, and every hour spent there prevents hours of
debugging strange behaviour later.

## Tools need contracts

When an agent calls a tool, the interaction should look like an API with a
schema: explicit inputs, validated outputs, and predictable failure modes.
We gave every tool a typed interface and made failures return errors the
agent could reason about, rather than silent empty results. An agent with a
broken tool is not lazy; it is misinformed, and it will confidently act on
that misinformation.

## Budgets and retries are policy

Employees do not get unlimited attempts and neither should agents. We capped
the number of tool calls and the total cost per request, and we designed
retries to happen at the right level. An automatic retry after a transient
failure is good; an agent looping between two tools until the budget burns
is a bug, and the budget is what turns that bug from a mystery into a logged
event.

## Verify before you trust

The manager does not take the employee's word for everything. We evaluated
agents on a fixed set of tasks with judged outcomes, including the cases
where the correct behaviour was to refuse. Running that set on every prompt
or tool change caught regressions that casual manual testing always missed.

## Not everything should be an agent

The hardest discipline is knowing when a plain function, a simple retrieval,
or a human is better. Agents shine when a task needs reasoning over tools and
unpredictable inputs. For everything else, an agent adds cost, latency, and
failure modes without adding value. The best agent project I worked on spent
more effort deciding what the agent should not do than deciding what it
should.

## Latency is a feature

One consequence of the employee model that teams forget is time. An
employee who takes forty minutes to answer is not useful, even if the answer
is perfect. Agents face the same constraint: users tolerate a few seconds,
not minutes of silent tool-calling.

We designed for latency from the start: parallel tool calls where safe,
caching for repeated sub-questions, and hard budgets on the number of steps.
We also streamed intermediate state so the user could see the agent working,
which made the wait feel honest. Perceived latency is as real as measured
latency, and a progress indicator is cheaper than a faster model.

Treat the agent like a junior hire with a manual: clear scope, good tools,
limits, and review. It will still surprise you, but the surprises become
manageable.
