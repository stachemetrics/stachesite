---
title: "Prototype #3: AstroShade"
date: 2026-03-19
draft: true
tags: ["42-26", "prototypes", "hair-colour", "AI", "gemini", "collaboration"]
---

This one wasn't my spreadsheet. Matt Smith — professional colourist — came to me with a question: could AI reason about hair colour formulation well enough that a stylist would actually trust it during a consultation? We built AstroShade to find out.

**[Try the demo](https://geoffpidcock--astroshade-web-dev.modal.run/)** | **[Source code](https://github.com/stachemetrics/astroshade)**

<!--more-->

## What it does

AstroShade walks through a salon colour consultation step by step — the way a colourist actually works:

1. **Client describes what they want** — text or a reference photo
2. AI analyses the desired look and the stylist confirms it understood correctly
3. **Stylist photographs the client's current hair**
4. AI analyses the starting point — existing colour, condition, level — and the stylist confirms
5. **AI generates a visual preview** of the expected result
6. **AI recommends a Goldwell formulation** with reasoning — products, ratios, timing, and developer strength
7. Optional email capture for follow-up

It's scoped to Goldwell product lines. There are other products, and its a place to start with some clear test cases thanks to Matt. The idea is that a working colourist could run through this with a client sitting in the chair, using it as a second opinion rather than replacing their judgement.

## How it works

The engine is Gemini — `gemini-2.5-flash` for text and vision analysis, `gemini-2.5-flash-image` for generating the preview. Each step has its own structured prompt with Pydantic schemas enforcing the output format, so the AI can't waffle — it has to commit to specific colour levels, undertones, and product recommendations.

Matt supplied the domain knowledge: test cases with example before/after photos, expected formulations, and the kind of shorthand a colourist actually uses. That became the few-shot examples baked into each prompt.

UI is Gradio, hosted on Modal — same stack as the previous prototypes. The multi-step wizard layout is new though, which Gradio handles reasonably well for a quick build.

## What I learned

**Domain expertise changes everything.** The first two prototypes — [SniffTester](/posts/snifftester/) and [TenderTrawl](/posts/tendertrawl/) — were domains I understood well enough to evaluate the output myself. Hair colour formulation? Not a chance. Without Matt's test cases and feedback on what "correct" looks like, I'd have had no way to know if the model was producing plausible recommendations or confident nonsense. This is the collaboration model that actually works for AI in specialised fields: the person who knows the domain steers, the person who knows the tech builds.

**The preview is the riskiest step.** Everything else in the pipeline is text analysis — the model reads an image or a description and produces structured output. The preview is generative: it has to *create* a realistic image of what the client's hair would look like after the formulation. Lighting differences between the input photo and the generated preview can make a perfectly good formulation look wrong. And any wobbles in the earlier inference steps — misreading the starting level, misjudging undertones — compound by the time you're generating a visual. A silly preview could undermine confidence in the whole tool, even if the formulation itself is sound.

**Structured output is non-negotiable for professional tools.** Free-text recommendations would be useless here. A colourist needs specific product names, mixing ratios, developer volumes, and timing — not a paragraph of suggestions. Pydantic schemas via Gemini's `response_schema` parameter forced the model to fill in every required field. The output is still more verbose than the terse shorthand a colourist would jot on a consultation card, but the information is all there and structured consistently.

## The hypothesis

Can foundation models reason well enough about hair colour to be useful in a real consultation? The answer looks plausible — the formulation recommendations match what Matt would expect for the test cases, and the structured workflow keeps the stylist in control at every step. But "looks plausible in testing" and "earns trust from working salon professionals" are different things. The real test is putting it in front of colourists and seeing whether they'd actually use it.

## The verdict

AstroShade is the first prototype in the [42-26](/posts/hello-world/) series built with someone else's expertise at the centre. Matt brought the domain knowledge that made it possible — the test cases, the product knowledge, and the critical eye for whether the output made sense. That collaboration pattern is worth more than the tool itself.

The formulation engine works. The preview needs more resilience. And the only way to validate either is to get it into a salon.

Prototype #3: shipped. With a collaborator this time.
