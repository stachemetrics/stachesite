---
title: "Prototype #2: TenderTrawl"
date: 2026-02-27
draft: false
tags: ["42-26", "prototypes", "tenders", "AI", "gemini"]
---

I brought my own spreadsheet to this one. Finding relevant government tenders is a pain — most people either rely on email alerts that miss half the field, or spend hours manually searching AusTender. Planning for tenders is even worse. What if AI could do the heavy lifting?

**[Try the demo](https://geoffpidcock--tendertrawl-web.modal.run/)** | **[Source code](https://github.com/stachemetrics/tendertrawl)**

<!--more-->

## What it does

You describe your business in plain English — or paste a URL — and TenderTrawl searches Australia's government tender database and tells you what's open and where the money's been going.

Beyond the open tenders, it surfaces:

- **Spending insights** — which agencies are buying what you sell, and how much they've spent historically
- **Supplier landscape** — who you'd be competing against, and how dominant they are
- **Contracts coming due** — expiring contracts worth and their value in the next six months, which means upcoming opportunities to get on panels before they re-tender

The idea is to hook with the free stuff — open tenders and market intelligence — and convert towards paid tender drafting once someone's found an opportunity worth pursuing. The "Draft a tender response" button appears after results load, which is the natural next step.

## How it works

The dataset is ~81,000 contract notices from AusTender spanning the past year — about $969 billion across 127 Australian government agencies and 24,000+ unique suppliers. That lives in a combined CSV, assembled from weekly AusTender exports.

The conversation flow runs in two tracks simultaneously:

1. **Insights** — pandas queries against the contract dataset: top agencies, dominant suppliers, expiring contracts by category
2. **Open tenders** — Gemini 2.5 Flash with Google Search grounding, finding live tender listings from AusTender and related procurement sites

Both streams feed into a single chat response. The UI is Gradio, hosted on Modal — same stack as SniffTester, same quick setup.

## What I learned

**The speed problem is real, and different here.** SniffTester is slow because it runs sequential source evaluations. TenderTrawl is slow because search grounding itself takes time, and pandas queries on 81K rows in a flat CSV add more latency on top. The honest fix isn't pre-computing for every possible business description — you can't enumerate those. It's moving the dataset into something query-optimised (DuckDB, SQLite) so the insight lookups are fast regardless of what the user asks, and letting search grounding handle the live tender side independently.

**The conversion mechanic needs to be earned.** The "Draft a tender response" button is only useful if the user has found something worth bidding on. That means the quality of the tender matching has to be high enough that someone actually wants to click it. Right now the match quality is good for obvious queries and shakier for niche ones — which is where the real SME opportunities usually live.

**"Demo in a day" is a real pitch.** The thing I kept coming back to while building this: a working prototype that demonstrates a business model — hook with free intelligence, convert to a paid service — is genuinely compelling in a way that a slide deck isn't. TenderTrawl isn't a finished product, *but it's a convincing argument for one*. That's the point.

## The verdict

The core loop works: describe what you do, get relevant tenders and market intelligence back. It's slow, and it'd need real data cleaning and caching before you'd put it in front of paying customers. But as a demonstration that AI can meaningfully help SMEs navigate government procurement — without a procurement consultant or a $500/month platform — it holds up.

Prototype #2: shipped. The spreadsheet is in the bin. Mostly.
