# Debugger – Custom Coding Agent

## Overview

**Debugger** is a custom-built coding agent designed to **analyze, diagnose, and fix coding errors** in a structured, reliable, and engineering-focused way

Instead of generating free-form answers, the agent follows a **strict response contract** and produces **deterministic, review-friendly outputs**.  
It behaves like a senior software engineer who carefully understands the full project context before suggesting fixes.

This agent was built as part of a hackathon  **Build coding agents using your prompt engineering skills.**.

---

## Agent Name

**debugger**

---

## Model Configuration

```yaml
model: Grok Code Fast 1
temperature: 0.2
tools:
  read: true
permissions:
  write: false
