---
layout: single
title: "My Capgemini Internship Experience"
date: 2025-02-13 09:30:00 +0530
categories:
  - full-stack
tags:
  - genai
  - clang
  - ast
  - capgemini
  - productivity
  - python
  - lsp
  - unit-testing
toc: true
classes: wide
---

## Introduction

From October 2024 to February 2025, I had the opportunity to intern with **Capgemini’s GenAI team** in Pune. Over the course of 4 months, I worked on building a **unit test case curator for C and C++ codebases** that leveraged **Clang AST, Language Servers (LSP), and Generative AI**.

The goal of the project was to boost developer productivity by creating a tool that could automatically generate meaningful unit test cases for any C/C++ function by analyzing the source code and passing relevant context to Capgemini's GenAI engine.

Here’s a chronological log of the milestones achieved during my internship.

## 📅 Timeline of Work

### ✅ 14 / 10 / 24 — Onboarding and Device Collection
On my first day, I completed all formalities related to onboarding. I was issued my work laptop and introduced to internal systems and tools at Capgemini.

---

### ✅ 20 / 10 / 24 — Orientation and HR Activities
This day was dedicated to HR inductions, compliance training, and learning about Capgemini’s core values and culture. It was great to meet fellow interns and understand the scope of various verticals.

---

### ✅ 24 / 10 / 24 — Manager Assignment Process
I was officially assigned to the **GenAI team**. I received the name of my reporting manager and was added to internal channels and sprint boards.

---

### ✅ 01 / 11 / 24 — Project Knowledge Transfer (KT)
A colleague walked me through an existing prototype and explained Capgemini's internal GenAI socket. We discussed expectations and the current state of the tooling.

---

### ✅ 05 / 11 / 24 — Research: Regex-based Parsing
I initially explored using regular expressions to parse code and extract function details. This turned out to be fragile and error-prone for nested or complex C/C++ structures.

---

### ✅ 11 / 11 / 24 — Research: GenAI-Based Context Extraction
Shifted focus toward using **GenAI to understand code context** instead of regexes. I started exploring how semantic understanding could make the tool more intelligent.

---

### ✅ 15 / 11 / 24 — Manager Sync: My Approach Using AST
I pitched my idea of using **Abstract Syntax Trees (AST)** via **Clang tooling** to parse C/C++ functions and extract precise context. The manager gave green light to proceed.

---

### ✅ 21 / 11 / 24 — Team Meeting: Tech Architecture
Presented a detailed technical architecture to the broader GenAI team, discussing how we'd gather function metadata, handle includes, dependencies, and socket communication.

---

### ✅ 28 / 11 / 24 — Presentation: My Approach Walkthrough
I gave a structured walkthrough of my AST-based context extractor, demonstrating early results and how we’d connect it with the GenAI engine for test case generation.

---

### ✅ 01 / 12 / 24 — Research: Clang AST
Deep-dived into Clang’s AST APIs, experimented with traversal techniques to extract function arguments, local variables, return types, and dependencies.

---

### ✅ 10 / 12 / 24 — MVP Presentation
Presented the MVP of the **Unit Test Curator Tool** to my manager. It could now extract clean context blocks for individual functions and output them as structured data.

---

### ✅ 15 / 12 / 24 — GenAI Socket Update
Collaborated with the core team to modify the socket protocol to better support test case suggestions and handle varied output lengths from the GenAI engine.

---

### ✅ 01 / 01 / 25 — MVP Integration with Code Tester
We integrated the context-extraction module with Capgemini’s internal code-testing framework. Test cases were now being generated in real-time via the GenAI socket.

---

### ✅ 15 / 01 / 25 — Final Tool Integration with GenAI Socket
The full pipeline was in place: from AST context extraction → passing to GenAI → receiving test cases → injecting into testing scaffolds. The tool was ready for team-wide use.

---

### ✅ 01 / 02 / 25 — Final Testing & Debugging
Conducted thorough debugging and ran validations on 100+ C/C++ functions. Edge cases, macros, and template handling were tested and optimized.

---

### ✅ 10 / 02 / 25 — Final Demo to Senior Director
I showcased the tool in a final demo session to Capgemini’s Senior Director. Received great feedback on the real-world productivity boost the tool offered to developers.

---

### ✅ 13 / 02 / 25 — Last Day and Device Handover
Wrapped up all documentation, submitted my project report, and returned the device. A wonderful learning journey came to an end. ✨

---

## 🧠 Tech Stack & Tools Used

- **Languages**: Python, C++
- **Compiler Tools**: Clang, LibTooling
- **AST Parsing**: Clang Python Bindings
- **GenAI Integration**: Capgemini GenAI Socket
- **Code Analysis**: Language Server Protocol (LSP)
- **Internal Testing Frameworks**

---

## Final Thoughts

This internship at Capgemini was more than just a corporate stint—it was an intense learning experience. I got hands-on with real-world GenAI applications, explored compiler internals, and improved my understanding of scalable tooling for enterprise codebases.

The project taught me how traditional techniques like ASTs can work hand-in-hand with next-gen GenAI models to solve developer problems more intelligently.

I’m incredibly grateful to my manager and team for the support and the freedom to explore ideas. Looking forward to what comes next! 🚀
