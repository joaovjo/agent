---
name: arch-review
mode: subagent
---
**Architecture Review: Scan codebase for improvement opportunities**

Use when: analyzing codebase architecture, before major refactoring, during code review, or when code quality is degrading.

**Process**:
1. **Scan**: Read key modules, analyze dependency graphs, measure coupling/cohesion
2. **Identify**: Deepening opportunities, structural issues, tech debt
3. **Report**: Prioritized list of improvements with impact estimate
4. **Discuss**: Grill through each opportunity with the user
5. **Plan**: Create ADR + implementation plan for chosen improvements

**What to Look For**:
- God modules (too many responsibilities)
- Tight coupling between unrelated modules
- Shallow modules (complex interface, little behavior)
- Circular dependencies
- Global mutable state
- Error handling gaps
- Missing abstractions
- Premature abstractions (YAGNI violations)
- Inconsistent patterns across the codebase

**Rust-Specific Checks**:
- `unwrap()`/`expect()` without justification (unsafe error handling)
- Large `match` statements that should be abstracted
- Missing `Result`/`Option` propagation
- Traits with too many methods
- Overly broad public API surface
- Missing `#[non_exhaustive]` on public enums
- `unsafe` blocks without safety comments
- Deeply nested closures

**Output Format**:
```markdown
# Architecture Review: [Project/Module]

## Overview
- Score: X/10
- Key finding: ...

## Opportunities (Prioritized)
| # | Issue | Severity | Effort | Impact | Module |
|---|-------|----------|--------|--------|--------|

## Deep Dive: [Top Issue]
### Current State
### Problem
### Solution
### Tradeoffs
### Implementation Plan

## Recommendations
1. ...
2. ...
```

**Integration with Agents**:
- architect: Leads the review, writes ADRs
- developers: Implements recommended changes
- qa: Validates no regressions from changes