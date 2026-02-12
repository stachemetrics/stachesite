---
title: "Prototype #1: SniffTester"
date: 2026-02-12
draft: false
tags: ["42-26", "prototypes", "fact-checking", "AI", "gemini"]
---

First cab off the rank for [42-26](/posts/hello-world/) — a tool that fact-checks AI-generated reports. Because if we're going to trust AI to write things, we should probably have AI double-check the homework... 🐢

**[Try the demo](https://geoffpidcock--snifftest-ui.modal.run/)** | **[Source code](https://github.com/stachemetrics/snifftester)**

<!--more-->

## What it does

You paste in an AI-generated report and SniffTester runs it through the [CRAAP framework](https://en.wikipedia.org/wiki/CRAAP_test) — an academic evaluation method that tests sources for:

- **Currency** — Is the information recent enough?
- **Relevance** — Does it actually relate to the claims being made?
- **Authority** — Is the source credible?
- **Accuracy** — Can the claims be verified?
- **Purpose** — Why does this source exist? (Informing, selling, persuading?)

It pulls out every reference and claim, then evaluates each one against these criteria using live web search. The output is a structured breakdown of what checks out and what smells funny.

## How it works

Under the hood it's Gemini with Google Search grounding, running two passes:

1. **Extract** — First prompt pulls out all the references, claims, and citations from the report
2. **Evaluate** — Second prompt takes each extracted reference and evaluates it against the CRAAP framework, using search results as evidence

The demo UI is built with Gradio, hosted on [Modal](https://modal.com). Quick to stand up, no infra to babysit.

## What I learned

**Source verification is slow.** Each reference needs its own search and evaluation pass. For a report with 15-20 citations, you're waiting a while. The obvious fix is parallelising the evaluation calls — right now they run sequentially, which is the low-hanging fruit for v2.

**Context matters more than I expected.** A source might be authoritative for one claim and irrelevant for another. The CRAAP framework handles this well in theory, but getting an LLM to consistently evaluate *in context* rather than just "is this a good source generally?" took some prompt iteration.

**The UX needs rethinking.** Right now you get a line-by-line evaluation — paste report in, get assessment out. But what you actually want is something like Word's track changes or Grammarly's inline highlights. Flag the dodgy bits right there in the source text so you can see *where* the problems are without reading a separate report. That's a harder UI problem than the AI part.

## The verdict

As a proof of concept, it works — you can catch hallucinated references and sketchy sources. But the "paste and wait" interaction model isn't how people actually want to verify documents. The next iteration would need to be an inline tool, and the evaluation would need to be fast enough to feel responsive. Parallelisation and caching would get it most of the way there.

Prototype #1: shipped. Learned things. Moving on.
