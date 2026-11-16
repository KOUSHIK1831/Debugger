# Debugger

**A structured coding-agent prompt for diagnosing errors and proposing reviewable fixes.**

Debugger is a hackathon project built around a reusable prompt contract rather than a standalone executable application. It guides an AI coding assistant to examine project context, identify likely root causes, and rank corrective actions without making unapproved changes.

## What it does

- Reviews supplied source files, error messages, and failing test output.
- Distinguishes likely root causes from secondary hypotheses.
- Recommends **three ranked fixes** with technical justification.
- Requests complete corrected code snippets in a consistent response format.
- Prioritizes minimal, safe changes and explicitly calls out missing context.

> [!NOTE]
> This repository contains the agent specification, not a packaged CLI or a deployed debugger. Results depend on the model, available project context, and whether its hosting environment supports the requested tools.

## Agent configuration

The source-of-truth prompt is in [`debugger.md`](debugger.md).

```yaml
name: debugger
model: Grok Code Fast 1
temperature: 0.2
tools:
  read: true
permissions:
  write: false
```

The specification uses a **read-only** workflow: it proposes changes for developer review instead of editing repository files.

## How to use

1. Open [`debugger.md`](debugger.md) and copy the agent instructions into an AI-agent platform that accepts custom prompts or agent definitions.
2. Select a compatible coding model and grant access to the relevant project files and error logs.
3. Ask the agent to diagnose an issue.
4. Review and test the proposed fixes before applying them.

**Example request**

```text
The test suite fails after a dependency upgrade.
Review the supplied files and stack trace, identify the root cause,
and rank three minimal fixes with corrected code.
```

## Diagnosis workflow

The [agent definition](debugger.md) requests a consistent sequence:

1. Examine the relevant files and their relationships.
2. Read the failing test, stack trace, or error message.
3. Separate likely root causes from possible secondary causes.
4. Consider multiple fixes and rank the **three most useful**.
5. Provide corrected code and explain why each alternative works.

The prompt deliberately requests targeted changes rather than unrelated
refactoring. Its agent configuration declares **read access without write
permission**, so suggested patches should be applied only after review.

## Example scenario

A caller might provide a TypeScript function and a failing unit test:

```text
Problem: A function returns undefined when an API response has no items.
Expected: Return an empty array.
Context: Here is the function, its type definition, and failing test.
Task: Diagnose the root cause and rank three safe fixes.
```

The requested output has an `Error Analysis` section and `Top Fixes`
with descriptions, rationales, and corrected code. The response may
still require additional project context before a diagnosis is reliable.

## Limitations and quality checks

- This repository contains a **prompt definition**, not an autonomous
  compiler, language server, or code execution environment.
- No executable test suite or CLI entry point is included.
- Read-only permission does not verify that a proposed patch compiles.
- A model cannot inspect files it has not been given or made able to read.

> [!TIP]
> Supply the exact stack trace, relevant function and types, and a
> minimal reproducing test. Evaluate the recommendations by running
> the affected tests and checking for regressions.

## Expected response

The agent prompt defines an output contract with an **Error Analysis** section and **Top Fixes** containing three ranked solutions. Each solution includes a description, a rationale, and corrected code. Consult [`debugger.md`](debugger.md) for the complete contract.

> [!IMPORTANT]
> Do not treat suggested patches as verified fixes. Review the changes and run your project's tests, especially for security-sensitive and production code.
