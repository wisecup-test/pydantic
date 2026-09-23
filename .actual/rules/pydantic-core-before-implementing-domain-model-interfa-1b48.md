# pyo3 Traversal and Conversion Protocol Integration for Domain Models: Before Implementing Domain Model Interfaces That

These rules are ALWAYS ACTIVE for domain model definitions, type serializers, schema validators, and lookup structures interacting with host runtime objects and participating in cyclic references across runtime boundaries.

### Rules

- **R-PYO3-001** MUST: Before implementing domain model interfaces that interact with pyo3 APIs, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify API availability against that version.

### Verify

```bash
# Discover and run the project test execution script from the repository configuration to execute domain modeling and boundary traversal tests.
# Discover and run the static analysis and linting script declared in repository metadata to verify traversal contract compliance across domain modules.
```

**Accept when:**
- All domain models crossing the runtime boundary implement required traversal protocols without reporting untracked reference leaks.
- All verification scripts pass without type conversion errors or memory safety violations.

<enforcement>
Claude Code MUST NOT skip or defer verification of pyo3 traversal contracts and dependency lock verification.
</enforcement>