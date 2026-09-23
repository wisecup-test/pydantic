# functools.lru_cache for Deferred annotated_types Constraint Mapping: Engineers Omit Maxsize Parameters Functools Lru

These rules are ALWAYS ACTIVE for internal metadata handling functions defining mappings between annotated types and internal constraint definitions, and performance-critical internal modules requiring deferred loading of external dependencies.

### Rules

- **R-FUN-001** MAY: Engineers MAY omit maxsize parameters on functools.lru_cache decorators when memoizing static, parameterless mapping functions.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>