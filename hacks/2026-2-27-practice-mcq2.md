---
layout: post
title: "AP CSP MCQ Reflection: What I Got Wrong & How I'll Improve"
date: 2026-02-27
author: Michelle Ji
tags: [AP CSP, computer science, studying, test prep, reflection]
description: A question-by-question breakdown of 14 missed MCQ answers on an AP Computer Science Principles practice test, with key takeaways and a study plan.
permalink: /mcq-reflection/
---


# AP CSP MCQ Reflection: What I Got Wrong & How I'll Improve

*14 questions missed (53/67). Here's what that actually means and what I'm doing about it.*

---

## The Pattern

Missing 14 questions wasn't random. Looking at the mistakes together, they fall into three clear buckets: **concepts I didn't know well enough**, **code I didn't trace carefully enough**, and **questions I just misread**. Each one points to a different problem — and a different fix.

---

## Mistake Type 1: Weak Conceptual Knowledge

Several questions exposed genuine gaps in my understanding of core CSP vocabulary. I confused TCP/IP with a security protocol (it's a communication standard). I thought Creative Commons had something to do with data transmission (it's a content licensing system). I didn't know that binary search only requires a sorted list. I mixed up lossless and lossy compression. I didn't connect overflow errors to fixed bit storage limits.

These aren't tricky questions — they're vocabulary. I lost these points because I half-knew the terms but couldn't distinguish the details under pressure.

**Concepts to drill:**
- Overflow (fixed bits → max value exceeded)
- Lossless vs. lossy compression
- TCP/IP (communication standard, not security)
- Public-key encryption (the right tool for securing transmitted data)
- Creative Commons (licensing, not transmission)
- Binary search (sorted list = the only requirement)
- Open-source software (no guaranteed support from original developers)

---

## Mistake Type 2: Not Tracing Code Carefully

A lot of my algorithm mistakes came from not working through the logic slowly enough. I picked answers that looked right but only handled some cases. I stopped checking once I found one correct answer instead of verifying all of them. I eyeballed execution times instead of actually counting procedure calls.

The pattern: I rushed. I found a plausible answer and moved on. For algorithm questions, that's a losing strategy.

**What to do differently:**
- Trace code with actual example values — write it out, don't visualize it
- For "select 2" questions, treat every answer choice as its own true/false before committing
- Count procedure calls explicitly when comparing execution times — don't estimate

---

## Mistake Type 3: Careless Reading

Some of these I genuinely knew but still got wrong. I left out user consultation on the program development question even though I know it matters. I picked the wrong compression type even though the question described it clearly. I selected a true advantage of open-source when the question asked for what is NOT an advantage.

These hurt the most because they're not knowledge gaps — they're focus gaps. I read too fast, or misidentified what the question was asking.

**What to do differently:**
- Flag key words before answering: "NOT," "best," "all of the following"
- Don't change an answer unless I can clearly explain why the new one is better

---

## What I'm Focusing On Next

**This week:** Flashcard every term in the concepts list above — not just the definition, but what each one is commonly confused with. TCP/IP vs. encryption. Lossless vs. lossy. Creative Commons vs. copyright.

**Also this week:** Redo every algorithm and code question from this test by hand, writing out each trace instead of doing it mentally.

**Next practice test:** Before submitting any answer, ask myself — "did I read the question carefully, and did I check every answer choice?" The score isn't the point right now. Understanding why I missed each one is.

---

*Questions missed: Q4, Q8, Q9, Q10, Q11, Q23, Q29, Q41, Q47, Q51, Q56, Q57, Q59, Q67*