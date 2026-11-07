---
layout: post
title: "Risk-Based Testing: Only Run the Tests That Matter"
date: 2026-11-06
tags: ["testing", "qa", "risk-based-testing", "microservices", "e2e", "contract-tests", "quality-engineering"]
summary: "With about 30 microservices, we do not run every test on every deploy. A risk matrix picks the impacted e2e and contract tests, and anything unmapped falls back to full regression."
---

I work in quality engineering, on a product with about 30 microservices. Running every e2e and contract test on every deploy is not a real option. It is slow, it is noisy, and it teaches the team to ignore the results.

So we stopped trying to run everything. We run the tests that matter for the change in front of us.

That is risk-based testing, and it is working very well so far.

## How it works

When a service is deployed or promoted to an environment, a workflow triggers a risk-based selection. It looks up a risk matrix, finds the services impacted by that change, and runs the e2e and contract tests for those services only.

The matrix is the core of it. It maps a change in one service to the other services and tests that can actually be affected by it. Contract tests check the boundary between services. E2e tests check the real flow across them. Together they cover the places where a microservice change actually breaks something, which is usually not inside the service itself, but where it talks to the rest of the system.

The result is a smaller, faster run, and a run the team actually reads. We cut the tests that had nothing to do with the change, and we kept the ones that did.

## The fallback matters more than the selection

A selector is only as good as its mapping, and mappings go stale. A new API route has no history. A dependency changes in a way no one wrote down. A service starts calling another service for the first time.

So we have a simple rule: if the risk is not mapped, or not proven, we run full regression to be safe.

That fallback is what makes the selective run trustworthy. Without it, a narrow run is just a guess. With it, the team knows that an unknown change gets the full check, and a known change gets the focused check. We save time on the changes we understand, and we do not save time on the changes we do not.

## This is the same idea Anthropic uses

Anthropic handles this at a very different scale. They merge more than 30,000 changes per day in their monorepo, with Claude writing roughly 80 percent of the code. They do not run all tests on every change either.

They use test impact analysis. A selector picks only the tests a change can affect, based on the code package and on past CI results, and they run the full suite on the default branch and in the merge queue.

The mechanism is different. Ours is an explicit risk matrix for microservices, triggered on deploy and promote. Theirs is automatic, built from package structure and CI history. But the principle is the same: run only the tests a change can affect, and keep a full run as the safety net for what the selector misses.

That is why I think this approach is not a shortcut. It is the same direction the field is moving, just applied to a smaller system.

## What is next: add history

Our matrix is maintained by hand, so it will go stale. The next step is to add history as a second signal, not as a replacement for the matrix.

History is the observed risk. It is what actually broke in the past, not what we thought would break. If a service change breaks a test for another service twice, that mapping should be in the matrix, even if no one wrote it down.

The plan is to start with logging only: record which service was deployed, which tests ran, and which ones failed. Review that log and use it to fix missing mappings. Then, once there is enough data, auto-include high confidence hits, for example a test that failed after the same service changed two or more times in the last 90 days.

Two rules stay in place. Filter out flaky tests first, or history will bloat the selection fast. And never auto-remove a matrix mapping just because history is quiet. No failures in the past does not mean safe. For new API routes, history has no data at all, so the fallback to full regression stays.

The matrix tells us what should be tested. History will tell us what we missed. Together they should make the selection both faster and safer.
