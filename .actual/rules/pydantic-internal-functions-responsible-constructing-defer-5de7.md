# functools.lru_cache for Deferred annotated_types Constraint Mapping: Functions Responsible Constructing Deferred Dependency Mappings

These rules are ALWAYS ACTIVE for internal metadata handling functions defining mappings between annotated types and internal constraint definitions, and performance-critical internal modules requiring deferred loading of external dependencies.

### Rules

- **R-ADR-001** MUST: Functions responsible for constructing deferred dependency mappings MUST apply functools.lru_cache to ensure that local import evaluation and dictionary allocation execute exactly once per process.

### Verify

```bash
# Discover the project test suite runner and execute all automated tests covering metadata collection and constraint mapping.
# Discover the project import benchmark scripts and verify that module import time meets established performance budgets without eager dependency loading.
```

**Accept when:**
- The deferred mapping function executes successfully and returns the expected constraint mapping without top-level import side effects.
- Subsequent invocations of the mapping function return the memoized dictionary without re-evaluating the local import.
- Automated test suites pass without regression in validation or schema generation pipelines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test suites, automated import tracking, and peer code review.
</enforcement>