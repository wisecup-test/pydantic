# pyo3 Traversal and Conversion Protocol Integration for Domain Models: Domain Entities Not Bypass Garbage Collection

These rules are ALWAYS ACTIVE for domain model definitions, type serializers, schema validators, lookup structures interacting with host runtime objects, and domain structures participating in cyclic references across the runtime boundary.

### Rules

- **R-PYO3-001** MUST_NOT: Domain entities MUST_NOT bypass garbage collection traversal by storing unmanaged host object references in internal domain collections.

### Verify

```bash
# Discover and run the project test execution script from the repository configuration to execute domain modeling and boundary traversal tests.
# Discover and run the static analysis and linting script declared in repository metadata to verify traversal contract compliance across domain modules.
```

**Accept when:**
- All domain models crossing the runtime boundary implement required traversal protocols without reporting untracked reference leaks.
- All verification scripts pass without type conversion errors or memory safety violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites, static analysis, and mandatory architectural code reviews.
</enforcement>