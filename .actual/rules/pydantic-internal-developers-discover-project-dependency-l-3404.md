# functools.lru_cache for Deferred annotated_types Constraint Mapping: Developers Discover Project Dependency Lock File

These rules are ALWAYS ACTIVE for internal metadata handling functions defining mappings between annotated types and internal constraint definitions, and performance-critical internal modules requiring deferred loading of external dependencies.

### Rules

- **R-ADR-001** MUST: Developers MUST discover the project dependency lock file and verify that the exact resolved version of annotated_types is confirmed prior to implementing or modifying constraint mapping definitions.
- **R-ADR-002** MUST: Define the internal mapping function with functools.lru_cache and contain the local import of annotated_types within the function body.
- **R-ADR-003** MUST: Verify that callers retrieve constraints through the memoized function rather than re-importing the target dependency directly.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test suites, automated import tracking/profiling checks, and peer code review.
</enforcement>