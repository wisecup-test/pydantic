# functools.lru_cache for Deferred annotated_types Constraint Mapping: Deferred Mapping Functions Encapsulate Associated Type

These rules are ALWAYS ACTIVE for internal metadata handling functions defining mappings between annotated types and internal constraint definitions, and performance-critical internal modules requiring deferred loading of external dependencies.

### Rules

- **R-METADATA-001** SHOULD: Deferred mapping functions SHOULD encapsulate all associated type-to-constraint translation logic within the memoized factory function rather than scattering localized imports.

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
Claude Code MUST NOT skip or defer verification. All metadata constraint generation changes must pass automated test suites and avoid unapproved top-level imports of deferred dependencies.
</enforcement>