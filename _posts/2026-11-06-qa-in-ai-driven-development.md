---
layout: post
title: "In AI-Driven Development, QA Is Not About Finding Bugs"
date: 2026-11-06
tags: ["testing", "qa", "ai", "llm", "requirements", "e2e", "quality-engineering"]
summary: "In AI-driven development, QA is not mainly about finding obvious bugs. It is about catching requirement drift and missing requirements."
---

I work in quality engineering. Over the last year I have spent most of my time on LLMs, agentic workflows, and building robust automated e2e test harnesses.

That work changed how I see QA. In AI-driven development, QA is not mainly about finding obvious bugs anymore. It is about catching requirement drift and missing requirements.

Obvious bugs are now the cheap part. An LLM can write code that compiles, passes linting, and passes its own unit tests. A null check, an off-by-one error, a syntax mistake, those get caught fast by the compiler, the linter, or a generated unit test.

The harder problem is code that works but does the wrong thing.

That is requirement drift. The spec said one thing, the prompt said something close to it, the agent interpreted it a third way, and three iterations later the code no longer matches what the user actually asked for. No test fails, because the tests were written from the same drifted understanding as the code.

Missing requirements are the same problem in the other direction. The requirement was never written down. It lived in a Slack thread, in a past decision, in an assumption about how two systems should work together. The AI did not miss it because it is careless. It missed it because it was never in the input.

When a requirement changes, the test cases need to change at the same time. They are one change, not two. If the code is updated and the tests are left alone, the tests stop describing what the product should do. A passing test can then lie, and a failing test can block code that is actually correct.

That is harder with AI-driven development. Code can now change in seconds, across many files at once, while tests lag behind. The requirement change is often buried in a prompt or a follow-up message, not in a tracked ticket, so there is no clear trigger to update the tests. And an agent can easily update the code without updating the tests, or worse, rewrite the tests to match the new code instead of the new requirement. Then everything is green, and nothing is checking the intent anymore. So the update has to be part of the same change: update the tests in the same change, remove the ones that no longer apply, and add the ones for the new behavior. Otherwise the drift just compounds with every iteration.

Anthropic is a good example of what this looks like at scale. They merge more than 30,000 changes per day in their monorepo, up from about 2,500 a day in May 2026, and Claude writes roughly 80 percent of the code. They do not handle that by adding more humans to update tests. There is no human QA gate per change.

Instead, they built the check into the system. They do not run all tests on every change. They use test impact analysis to run only the tests a change can affect, based on package and past results, and they run the full suite on the default branch and in the merge queue. The agent has to show runnable proof for its change, and a fresh review checks the diff. Human review focuses on intent and risk, not on reading every line. Low risk changes can merge with no human once automation is green. Higher risk changes get one or two people.

The safety net is also after merge, not only before it. They invested in post-merge and pre-deploy automated testing, and new behavior ships behind flags to employees first. A flag can be turned off without a deploy. When something still slips through, that incident or revert becomes an eval, and that eval set runs daily against the reviewer. The QA system itself gets regression tested.

That is the point. At 30,000 changes a day, you cannot rely on a person remembering to update the tests when the requirement changes. You have to make the agent prove the new behavior, make the review check intent, select only the affected tests to keep CI fast, and then learn from the failures that still get through.

This is why e2e tests matter more now, not less. Unit and integration tests are easy for current LLMs to generate. E2E is where cross-spec, cross-component, and cross-repo behavior shows up. It is where a feature can pass in isolation and still break the real flow. A unit test can be green while the product is broken for the user. An e2e test asks the question that matters: does this actually work end to end?

So the QA job shifts. Less time checking if the code runs. More time checking if the code still matches the intent.

That means asking different questions: Did this change match the current requirement, or an old version of it? What was assumed but never stated? What happens when this feature meets the rest of the system, not just its own test file?

AI made writing code faster. It did not make understanding requirements faster. That is still the work, and that is where QA earns its place now.
