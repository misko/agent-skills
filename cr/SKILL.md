---
name: cr
description: Read-only code review of named files, functions, or a behavior. Run isolated test cases and report correctness, efficiency, and recommended tests in plain language.
---

# Read-only code review

Treat the text following `/cr` as the review brief. Accept paths, symbols, a description such as “how we merge two recordings,” and any expected behavior, workload, or constraints. Resolve descriptions by tracing actual entry points and callers. If the request has no target, use a clearly identified change from the conversation or ask which behavior to review.

1. **Establish the contract.** Read the current working files, including relevant uncommitted changes. Trace one representative input to its output. Use caller expectations, documentation and existing tests to establish expected behavior. State assumptions when these disagree. Keep the review focused on what could affect the requested behavior.
2. **Protect the original.** Inspect the source in place using read-only operations. Execute project code and tests from an isolated temporary copy of the relevant working files outside the checkout. Include relevant local edits. Ensure imports, symlinks, configuration, build outputs, caches and test data cannot direct writes back to the original or to shared services. Prefer a read-only source mount with separate scratch storage when available. Use disposable fixtures for external dependencies. If safe execution is unavailable, continue with static review and identify exactly what remains unverified. Never edit, reset, stash, or clean the original checkout. Deliver the report in chat unless the user requests a file.
3. **Delegate when it adds coverage.** Review small scopes directly. For independent paths or questions, optionally assign subagents distinct files, functions, or hypotheses. Give each the same read-only boundary, separate scratch space, the relevant contract, and a request for source locations and observed results. The lead reviewer reconciles conflicting findings and verifies material claims.
4. **Exercise the behavior.** Design and run your own focused cases through the actual implementation. Include an ordinary case and a boundary or failure case relevant to the contract. Pick expected results independently, using a hand-worked example, invariant, or simple reference calculation. Record inputs, expected and actual outcomes, and how to repeat the check. Explain any failures caused by the harness. Distinguish tested behavior, source-based findings, and open questions.
5. **Assess efficiency at a relevant scale.** Identify the operations that grow with input size, including material allocations, I/O, or repeated work. If timing would change the recommendation, measure a representative workload in isolation, record size and conditions, and check that any scratch alternative produces equivalent outputs. Label estimated gains and their assumptions. Give no numerical speedup without supporting evidence.

## Report

Lead with the most consequential result. Use concise, ordinary language in the style of `/bro`; the reader should understand what goes in, what happens, and what comes out. Include:

- **What the code does.** Explain the behavior with a concrete example.
- **Issues and open questions.** Order by impact. Give each finding a file and line, its trigger, consequence, and evidence. For an unresolved concern, say what observation would settle it.
- **Checks run.** Show expected versus actual results and flag untested paths.
- **Efficiency and potential gains.** State the current cost, what drives it, and which change could matter for the stated workload.
- **Tests worth adding.** Recommend specific inputs and assertions, prioritized by the regression each would catch. Mention existing coverage where it already suffices.

When a category has no material findings, say so briefly. Leave implementation decisions to the user.
