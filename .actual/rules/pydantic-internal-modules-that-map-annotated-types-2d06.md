# functools.lru_cache for Deferred annotated_types Constraint Mapping: Modules That Map Annotated Types Not

These rules are ALWAYS ACTIVE for internal metadata handling modules defining mappings between annotated types and internal constraint definitions.

### Rules

- **R-28-001** MUST_NOT: Modules that map annotated types MUST NOT import annotated_types at global module scope.
- **R-28-002** MUST: Define internal mapping functions with functools.lru_cache and contain the local import of annotated_types within the function body.
- **R-28-003** MUST: Treat returned mapping dictionaries as read-only or construct immutable views to prevent mutation of the cached dictionary.

### Verify

```bash
# Discover the project test suite runner and execute all automated tests covering metadata collection and constraint mapping.
pytest tests/test_metadata.py -k "annotated"

# Discover the project import benchmark scripts and verify that module import time meets established performance budgets without eager dependency loading.
python -m pytest --import-mode=importlib
```

**Accept when:**
- The deferred mapping function executes successfully and returns the expected constraint mapping without top-level import side effects.
- Subsequent invocations of the mapping function return the memoized dictionary without re-evaluating the local import.
- Automated test suites pass without regression in validation or schema generation pipelines.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing top-level imports of deferred dependencies or non-memoized repeated local imports are blocked during review and static analysis.
</enforcement>