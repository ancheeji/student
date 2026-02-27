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

*A personal study blog after my latest practice test — 14 questions missed (53/67)*

---

## Q4 — Overflow Error
**What I missed:** I chose A (undecidable problem), but overflow has nothing to do with undecidability.  
**The right idea:** Overflow happens when two numbers are added and the result is too large to fit in the fixed number of bits the program uses to store integers.  
**What to remember:** Overflow = result exceeds max representable value in a fixed bit system.

---

## Q8 — Best Practices in Program Development
**What I missed:** I chose B (I and III only), leaving out consulting users.  
**The right idea:** All three practices are helpful — including consulting potential users. User feedback is a core part of good program development.  
**What to remember:** Never underestimate the importance of user consultation in development.

---

## Q9 — Transmitting Private Data Securely
**What I missed:** I chose B (high-bandwidth connection), but speed has zero relationship to security.  
**The right idea:** Public-key encryption (C) is the correct method for securing private data in transmission.  
**What to remember:** Encryption = security. Bandwidth = speed. They are completely separate concepts.

---

## Q10 — Science Museum Ticket Prices
**What I missed:** I chose C, which only adds +2 for people who are BOTH over 12 AND on a tour — missing several cases.  
**The right idea:** Answer B correctly adds +2 for age>12 independently, then adds another +2 for tours independently, covering all four ticket price combinations.  
**What to remember:** Trace every code segment against all possible inputs before choosing.

---

## Q11 — Binary RGB Triplet Color
**What I missed:** I chose B (light yellow), but 11111111 11111111 11100000 = (255, 255, 224) which is light yellow — not what was asked.  
**The right idea:** 11110000 in binary = 240 in decimal, so the triplet is (255, 255, 240) = Ivory (A).  
**What to remember:** Always fully convert each binary value to decimal before matching to the table.

---

## Q23 — Flowchart to Set `available`
**What I missed:** I chose B, which I misread as different from the flowchart — but I confused the AND/OR logic.  
**The right idea:** The flowchart requires BOTH conditions to be true (weekday AND miles < 20). I need to trace every branch carefully before picking an equivalent statement.  
**What to remember:** Always trace every branch of a flowchart — don't assume AND vs OR without checking.

---

## Q29 — Lossless Compression
**What I missed:** I chose B, which describes lossy compression (removing details the ear can't easily perceive — data is permanently lost).  
**The right idea:** Lossless (A) means the file is compressed AND can be fully restored to its original form on the other end.  
**What to remember:** Lossless = fully recoverable. Lossy = permanent data removal. These are opposites.

---

## Q41 — TrimLeft and TrimRight
**What I missed:** I chose A (statement I only), missing that statement III also works.  
**The right idea:** Statement III calls TrimLeft first (removes 11-char date), then TrimRight (removes 4-char ".jpg") — which produces the correct result just like statement I.  
**What to remember:** Work through string procedures inside-out and test each statement independently.

---

## Q47 — Requirements for Binary Search
**What I missed:** I chose B (no duplicate values), but duplicates don't break binary search at all.  
**The right idea:** Binary search only requires the list to be in **sorted order** (C). Duplicates, even length, and target value don't matter.  
**What to remember:** Binary search = sorted list. That's the one and only requirement.

---

## Q51 — Creative Commons
**What I missed:** I chose C, thinking it had something to do with reliable data transmission — completely wrong domain.  
**The right idea:** Creative Commons is a licensing system that lets creators specify how their work can legally be used and distributed.  
**What to remember:** Creative Commons = content licensing, not networking or transmission.

---

## Q56 — Comparing Execution Times
**What I missed:** I chose C (Version I takes ~1 more minute), but Version II is actually the slower one.  
**The right idea:** Version I calls GetPrediction once per element (4 calls). Version II calls it twice per element plus once more at the end (~9 calls). Version II takes about 5 more minutes.  
**What to remember:** Count every single call to slow procedures — including ones hidden inside conditions.

---

## Q57 — Purpose of TCP/IP
**What I missed:** I chose B (ensures private data is inaccessible to unauthorized users), but TCP/IP is not a security protocol.  
**The right idea:** TCP/IP establishes a **common standard** for how devices communicate over the Internet (C). Security is handled separately via encryption.  
**What to remember:** TCP/IP = communication standard. Encryption = security. Don't mix them up.

---

## Q59 — Advantages of Open-Source Software
**What I missed:** I chose D, but that's actually a TRUE advantage of open-source — it can be updated without the original developers.  
**The right idea:** The NOT an advantage is C — original developers of open-source software do not provide guaranteed free support.  
**What to remember:** Open-source = free to use/modify, but no built-in support obligation from the original creator.

---

## Q67 — Error in numOccurrences Procedure (Select 2)
**What I missed:** The correct answers were A and B. I chose B and D — getting B right but missing A and wrongly picking D.  
**The right idea:** Answer A also fails because the count resets incorrectly during iteration, returning the wrong value. I needed to trace through the code for every answer choice, not just the ones that looked obviously wrong.  
**What to remember:** For "select 2" questions, trace the code for ALL four options — don't stop once you find one that seems right.

---

## Overall Takeaways

- **Slow down and trace code manually** — Q10, Q41, Q56, Q67 were all lost to rushing
- **Convert binary to decimal fully** before doing anything — Q11
- **Know your vocab cold**: overflow, lossless vs lossy, TCP/IP, public-key encryption, Creative Commons, binary search requirements
- **For "select 2" questions**, evaluate every single answer choice independently

*Next step: flashcards for every bolded term above + one timed practice set by end of the week.*