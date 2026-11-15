---
layout: post
title: "Writing prompts is engineering, not poetry"
date: 2026-05-16
tags: [llm, prompts]
---

When prompting started, the advice was mystical: "be the model", "breathe
into the token", "write like a gentle mentor". Years of production LLM work
later, I can report the truth: prompting is just engineering with an
unreliable interpreter, and the same disciplines apply.

## The prompt is a specification

A good prompt states the task, the inputs, the output format, and the
failure behaviour. Exactly like an API spec. Vague instructions produce
plausible outputs; precise ones produce usable outputs. We treated prompts
like code and reviewed them like code, because they live as long as code and
break as often.

## Examples are the cheapest documentation

Two well-chosen examples teach more than a paragraph of explanation. We kept
examples short, diverse, and honest: one easy case, one hard case, one case
where the correct answer is "no". The failure example mattered most, because
the default behaviour of a model is to answer everything.

## Output contracts before creativity

Every production prompt ended with a strict format: JSON with a fixed
schema, or plain text with delimiters. The validation layer was not
optional. Creative freedom in output format is how you inherit a thousand
edge cases from a stranger's model.

## Version everything

Prompts drift: a word here, an example there, and the behaviour changes
silently. We versioned prompts with the code, logged which prompt served
which response, and re-ran evaluation sets on every change. A prompt change
is a model change, and it deserves the same ceremony.

## Separate the instructions from the data

Never let user input sit inside the instructions. We used delimiters and
explicit boundaries, and treated everything inside the data section as
untrusted. This is the prompt-injection hygiene that most teams learn the
hard way, and it costs two lines to get right.

## The eval-first habit

The habit that separated good prompting from guessing was simple: every
prompt change ran against a small evaluation set before it touched
anything else. A dozen representative inputs with judged outputs,
checked after every edit. The set was small enough to run by hand and
valuable enough that nobody shipped without it.

This changed the nature of prompt work. Instead of tweaking words and
hoping, we tweaked words and observed. A prompt that scored worse was
reverted, no discussion. Over time the evaluation set itself became
the specification, and the prompt became just the current best
implementation of it. That inversion is the moment prompting stops
being poetry and becomes engineering for real.

The mindset

The people who write the best prompts are not poets. They are engineers who
specify, version, test, and review. Approach the prompt like a contract with
a very literal, very forgetful colleague, and most of the mystery disappears.
