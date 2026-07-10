---
layout: post
title:  "Truth is not a direction: a Tarski attack on LLM probes"
date:   2026-07-10 10:00:00 +0200
author: Abel Jansma
tags: maths machine-learning category-theory
---


<br>

<br>

*A diagonal attack for LLM truth probes shows why no probe on a language model's embedding space can pin down truth.*

<br>

![](/assets/blogPics/TINAD_ourobouros.png)


<br>



## A linear dream

Modern LLMs famously encode input texts as vectors in some embedding space. One of the most satisfying discoveries about LLMs is that many natural concepts, like gender, emotions, capital cities, correspond to *directions* in this space. The extent to which a certain input text contains the concept “Male”, for example, can be quantified by the angle between the input’s embedding, and the direction corresponding to the “Male” concept. This is often referred to as the *Linear Representation Hypothesis*. 

<br>

I recently learned about a very bold version of this hypothesis, which claims that there is a direction that corresponds to “truth”. This is explored by Marks and Tegmark [here](https://arxiv.org/pdf/2310.06824) for example, and further studied [here](https://www.alignmentforum.org/posts/72vpkRRvoPHKi48fi/truth-is-universal-robust-detection-of-lies-in-llms-3), [here](https://www.lesswrong.com/posts/cmicXAAEuPGqcs9jw/how-well-do-truth-probes-generalise#Probe_algorithms), [here](https://arxiv.org/pdf/2212.03827), [here](https://arxiv.org/pdf/2310.01405), and [here](https://arxiv.org/pdf/2506.00823). 

Having access to such truth directions can be useful if you want to know if a piece of text is true, and crucial to AI safety research because it can reveal if an AI system is being truthful or deceitful.

AI safety researchers have thus been feeding LLMs true and false statements, and training classifiers that separate their embeddings. This works weirdly well, and seems to generalise to some extent. Could it really be true, that a superhuman AI could reflect the truth of propositions in the embedding geometry?

<br>

You might recognise this dream from elsewhere. Russell, Whitehead, and Hilbert had hoped to construct something similar for all of mathematics: a systematic way to decide on truth. In their case, it was based on the machinery of mathematical proof. However, Gödel famously destroyed that dream: no such systematic way to prove or disprove any mathematical statement can exist. Mathematical truth can not be fully captured by provability. 

<br>

## Speaking of "*speaking of*"
Gödel showed this by creating a clever system in which arithmetic expressions could state things about arithmetic sentences. This then created the ouroboros sentence `this sentence has no proof`. Tarski sharpened the paradox further: **no sufficiently expressive language can contain its own total truth predicate.** If a language is expressive enough to describe its own semantics, then `is true` as it applies to statements in that language, cannot fully be spoken of from *within* it.

<br>

Turing’s halting problem provides another example of such a *diagonal* construction: there is an evaluator (the program `HALT`), a system expressive enough to ‘talk about itself’ (descriptions of Turing machines can be fed into Turing machines), and a negation. A nice universal approach to such paradoxes of self-reference is [this](https://arxiv.org/abs/math/0305282) paper by Noson Yanofsky, who says

> *All these different examples are really saying the same thing: there will be trouble when things deal with their own properties.*


Note that transformer-based LLMs do something similar: they represent inputs as vectors in a space where directions correspond to concepts. However, these concepts can themselves be expressed in natural language that can be fed as input! Transformers trained on natural language are *made of the same stuff as their inputs*. Not all geometry of these embedding spaces corresponds to easily verbalisable structure, but important parts do (see for example Anthropic's post on [J-space](https://transformer-circuits.pub/2026/workspace/index.html)), and in particular the truthfulness concept often seems present. 

<br>

## A diagonal attack
Now let’s set up the attack. Let `t(s)` be the output of the truth probe on the input string `s`. This could be the projection of the model’s embedding on the truth-direction for example, or a non-linear classifier, it doesn’t really matter. Then consider

`t(“The truth probe's score for this sentence evaluates to FALSE.”)`

<br>

What should this evaluate to? If the sentence is true, then a correct probe should output `TRUE`. However, if the sentence is true, then according to the sentence the truth probe will output `FALSE`, which would indicate that the sentence is actually false. If the sentence is false, then apparently the truth probe will output `TRUE`, etc... A paradox.

Clearly, no such universal truth probe can exist! More generally:

<br>

> **No definable probe over a model's representation space can exactly capture truth for any language rich enough to describe that probe and its outputs.**

<br>

Note that "rich enough to describe that probe and its output" is a very low bar. It just means the language can name the model, the probe, and form sentences about their behaviour. English certainly can. 

What’s fun about this example, however, is that we can just see what happens when we do try to evaluate this!

<br>

I created a simple truth-probe for the `Qwen3.5-4B` model by training a logistic regression classifier on projections on the difference in mean embeddings of a set of training sentences that are labelled `TRUE` or `FALSE`. This difference-in-means approach was shown [here](https://www.alignmentforum.org/posts/72vpkRRvoPHKi48fi/truth-is-universal-robust-detection-of-lies-in-llms-3) to work well, and indeed, after training on 120 labelled example sentences, the probe (thresholded at 0.5) is 94% accurate on 36 withheld evaluation sentences (AUC of 0.98, the only mislabeled sentences being arithmetical ones like `five times seven is thirty-five`).

(**Note: this is obviously a very small toy example, only intended to illustrate that the diagonal Tarski attack can be executed in practice.**)

<br>

![](/assets/blogPics/TINAD_val_scores.png)

<br>

On diagonal attack sentences, however, the score seems to be nonsensical and all over the place. This is not just an effect of self-reference. Some self-referring sentences have clear truth-values, like `This sentence is written in English`, which the model scores accurately. 

<br>

![](/assets/blogPics/TINAD_diagonals.png)

<br>

## A Fixed-point fix?
All these liar-paradox like scenarios are examples of Lawvere’s fixed-point theorem, which roughly states that when a system contains a sufficiently expressive evaluator, every self-map on the target of the evaluator (i.e. functions on the set of ‘truth values’), must have a fixed point. Since negation on the set `{TRUE, FALSE}` does not have a fixed point, no such evaluator can exist. But what if we construct a set of truth values where all operations *do* have fixed points? Would that solve the paradox and allow for a universal *generalised* truth probe?

<br>

Let's represent `{TRUE, FALSE}` as `{0, 1}`. Negation can then be represented as a map `v -> 1-v`, which has no fixed point. However, if we extend the domain to the full interval `[0, 1]`, then negation has a fixed point at `1/2`. The standard liar paradox sentence can then be expressed as

<br>

`t(“This sentence is not true.”)`

<br>

Suppose its truth value is `x`. That means that it is true with value `x`, and therefore should receive a score of `1-x`. This is a fixed point equation, which has a solution at `x = 1/2`. So the standard liar sentence would get a self-consistent valuation of 0.5.

This resolved the most basic liar paradox, but not every diagonal attack, since not *all* functions on `[0, 1]` have fixed points. To make this work in general, we can for example allow only continuous functions on `[0, 1]` (which always have a fixed point by Brouwer's fixed-point theorem). But that restriction comes at the cost of expressivity: `"This sentence has truth score less than 0.5"` is not a continuous function of the truth score of the sentence. Accordingly, it leads to a new liar paradox in the graded truth semantics. There is no satisfying protection against these diagonal attacks, and no such universal truth probe can exist. 

<br>

## Takeaways
- Truth probes that project on a single direction work surprisingly well in simple cases. 
- However, they are *not* truth oracles, and never will be.
- A graded truth semantics on `[0, 1]` with continuous truth operations can assign the ordinary liar the fixed-point value `1/2`, but only by restricting crisp assertions about exact truth scores.
- Conventional logistic truth probes do not implement such reflective semantics merely because their outputs lie in `[0, 1]`.

<br>

It might seem absurd to you to even suggest superhuman AIs could function as a truth-oracle (it certainly does to me), but there are two reasons to take it seriously. First, it is how these things will be used *practically* by the vast majority of people. They are already replacing standard Google search results, and I’ve had many discussions end with people delegating final authority on the truth to an AI. Second, some people do really believe in a kind of [platonic representation space](https://arxiv.org/abs/2405.07987) that all models converge on, and that represents the “true” state of the world. If truth is indeed an objective part of the world, then you might expect such a universal truth direction to emerge as models get better. This post argues that this would lead to paradoxes. 

<br>

Finally, I want to emphasise again that I'm not claiming that truth probes are not useful! I think they could greatly help with understanding model behaviour, and picking up on misaligned behaviour early. Mathematics has not seriously been hindered by the problem of undecidability or the undefinability of truth. The fact that truth is not a direction in an AI's embeddings space similarly won't stop us from making progress in AI alignment research.
