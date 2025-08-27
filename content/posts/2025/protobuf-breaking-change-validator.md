---
title: "Protobuf Breaking Change Validator"
author: "Cássio"
authorAvatarPath: "/profile.jpeg"
date: "2025-01-22"
summary: "Proof of concept of automated breaking change validation on protobuf schemas."
description: "Proof of concept of automated breaking change validation on protobuf schemas."
toc: false
readTime: true
autonumber: false
math: false
tags: ["protobuf", "schema", "schema-validation", "events", "event-driven-architecture", "event-based-systems"]
showTags: true
hideBackToTop: false
fediverse: "@cassiofariasmachado@mastodon.social"
---

> ℹ️ This article was originally published in Portuguese on LinkedIn. You can read the original version [here](https://www.linkedin.com/posts/cassiofariasmachado_ol%C3%A1-rede-nas-%C3%BAltimas-semanas-me-deparei-activity-7288224039737696256-S986).

Hello everyone! 👋

In the last few weeks, I came across an interesting challenge and would like to share what I learned.

### ⚠️ Problem

In the context of event-based systems, how can we ensure there are no compatibility breaks when working with schemas using Protobufs? Recently, this question arose in the team I work with.

### 💡 Insight

An automated validation would be ideal to prevent changes that could break event compatibility. This type of compatibility is known as `backward compatibility`.

### ✅ Solution

After some research, we found options like [Buf CLI](https://buf.build/docs/cli), which offers command-line tools to validate and prevent compatibility breaks. In addition to other features, such as lint and Git integration, which greatly simplifies the workflow.

### 📌 Proof of concept

Here is a proof of concept (POC) using this approach, running via GitHub Actions and adding feedback directly on Pull Requests:

![Proof of concept example](/posts/2025/protobuf-breaking-change-validator/proof-of-concept-example.png)

### 📚 References

- [Buf CLI](https://buf.build/docs/cli);
- [Proof of concept Repository](https://github.com/cassiofariasmachado/protobuf-breaking-change-validator).