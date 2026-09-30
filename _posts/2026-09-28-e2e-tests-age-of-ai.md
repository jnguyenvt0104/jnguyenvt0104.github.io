---
layout: post
title: "Why E2E Tests Matter More in the Age of AI"
date: 2026-09-28
draft: true
tags: ["testing", "e2e", "ai", "llm", "quality-engineering"]
summary: "Unit and integration tests are easy to generate now. E2E is where the real value is, because LLM-generated code breaks at the seams."
---

I'm in quality engineering. Over the last year I've gone deep on LLMs, agentic workflows, and one specific problem: how to build a robust automated e2e test harness.

Here's my thesis. Unit and integration tests are already easy. Current LLMs generate them well. E2E is where the real value is now.

The weakness of LLM-generated code is the seams: cross-spec, cross-component, cross-repo. Even with spec-driven development, each piece can look correct in isolation while the whole thing quietly fails. Unit and integration tests pass. E2E catches it.

I saw this firsthand building a test harness for a Skyvern workflow, layered the old-fashioned way: contract tests first, then e2e. Reviewing LLM-generated code and specs is exhausting for humans. Bugs hide in plain sight. But e2e tests written against a risk map (release surface area mapped to risky components, tests tagged to that coverage) caught embarrassingly simple mistakes. The kind you'd never expect a "smart" model to make.

My favorite example: a workflow that interpreted age from a birth year. The model couldn't do the age math. The entire workflow was useless. Nothing below e2e flagged it.

Meanwhile the SDLC keeps accelerating. Generated code, generated tests, everything faster. And counterintuitively, manual testing feels more valuable than ever. It's the last line of defense against the obvious hallucination-driven breakage that automation and tired reviewers both miss.

The pyramid hasn't collapsed. But in the AI age, the top of it is doing the heavy lifting.
