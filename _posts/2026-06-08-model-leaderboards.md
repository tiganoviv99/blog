---
layout: post
title: "Why I stopped believing in model leaderboards at work"
date: 2026-06-08
tags: [evaluation, career]
---

Early in my career I treated leaderboards like verdicts: the model with the
best number won, end of discussion. Years of production work later, I
realise a leaderboard is the beginning of the conversation, not the end.
Here is why I stopped believing in them, and what I use instead.

## The test set is a snapshot of the past

By the time a model tops a leaderboard, the world has moved. Users ask new
questions, the catalogue changed, the season turned. A leaderboard tells you
how a model performed on data that already existed. It says nothing about
data that does not exist yet.

## The metric is a political choice

Someone chose that metric over five others. Accuracy hides cost asymmetry;
NDCG hides position bias; recall hides precision disasters. A leaderboard is
not objective reality. It is a committee decision encoded as a number, and
the number inherits all the committee's blind spots.

## The delta is what matters, not the rank

A 0.3 point difference at the top of a leaderboard is usually noise, not
signal. We started reporting confidence intervals and running repeated
evaluations with different seeds and splits. Half the "wins" evaporated. The
other half became real, and those were worth acting on.

## Production has its own scoring function

The final judge is users, and users do not care about the leaderboard. They
care about latency, cost, and whether the answer was useful. A model that
scores slightly lower offline and is twice as cheap to serve is usually the
right product decision. The leaderboard cannot see that trade-off.

## What replaced the leaderboard for me

Three things: a trusted evaluation set with slices that reflect the
business; a small set of regression checks that must pass; and an A/B test
for anything that survives those. The leaderboard still exists, but it is a
filter, not a podium.

## The paper-reading fix

The healthy version of following the field is reading papers for
ideas, not for verdicts. I read to steal one thing: a framing, a
trick, a way of thinking about a problem I already have. Then I close
the paper and let it sit. If the idea still maps to the problem a week
later, it goes into the queue; otherwise it was entertainment, which
is also fine.

This is slower than the hype cycle and faster than the hype cycle at
the same time: slower to adopt, faster to be right. The point of
reading is not to keep up. It is to have better options when a real
problem shows up. Papers are a pantry, not a parade.

Stop treating evaluation numbers as a ranking of models. Treat them as a
map of trade-offs. The best model is the one that fits the business, and the
map will show you where it is.
