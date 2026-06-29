---
name: tdd
mode: subagent
---
**TDD: Test-Driven Development for Rust**

**Briefing**:
Use red-green-refactor loop for all new features and bug fixes. Write failing test first, make it pass, then refactor.

**When to Use**:
- New feature development
- Bug fixes (write regression test first)
- Refactoring with safety net
- When test coverage is below threshold

**Workflow**:
1. **RED**: Write a failing test that defines the desired behavior
2. **GREEN**: Write minimal code to make the test pass
3. **REFACTOR**: Improve code while keeping tests green

**Rust-Specific Practices**:
- Use `cargo test` for unit tests
- Use `cargo test --test integration` for integration tests
- Use `testcontainers` for database tests
- Use `proptest` for property-based testing
- Use `mockall` for mocking (when needed)
- Target: >90% coverage for critical paths

**Test Organization**:
```
tests/
├── unit/           # Pure unit tests
├── integration/    # Cross-module tests
├── e2e/            # End-to-end tests
└── bench/          # Performance benchmarks
```

**Anti-Patterns to Avoid**:
- Testing implementation details
- Flaky tests (non-deterministic)
- Over-mocking (test real behavior)
- Skipping tests for "trivial" code
- Writing tests after code (not TDD)

**Integration with Agents**:
- swe-frontend: UI component tests, E2E
- swe-backend: API tests, DB tests
- swe-infra: Pipeline tests, deploy tests
- qa: Validates TDD cycle was followed