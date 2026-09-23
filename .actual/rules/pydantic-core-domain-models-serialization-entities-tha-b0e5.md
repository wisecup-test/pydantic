# pyo3 Traversal and Conversion Protocol Integration for Domain Models: Domain Models Serialization Entities That Hold

These rules are ALWAYS ACTIVE for all domain models, type serializers, schema validators, lookup structures, and serialization entities that hold references across runtime boundaries.

### Rules

- **R-PYO3-001** MUST: Domain models and serialization entities that hold references across runtime boundaries MUST implement traversal and conversion interfaces using pyo3 contracts to participate in garbage collection and object lifecycle tracking.

### Verify

```bash
# Discover and run the project test execution script from the repository configuration to execute domain modeling and boundary traversal tests.
# Discover and run the static analysis and linting script declared in repository metadata to verify traversal contract compliance across domain modules.
```

**Accept when:**
- All domain models crossing the runtime boundary implement required traversal protocols without reporting untracked reference leaks.
- All verification scripts pass without type conversion errors or memory safety violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites executing domain traversal and lifecycle verification scripts in continuous integration pipelines and mandatory architectural code reviews.
</enforcement>