# pyo3 Traversal and Conversion Protocol Integration for Domain Models: String Representations Mapping Accessors Domain Models

These rules are ALWAYS ACTIVE for domain model definitions, type serializers, schema validators, lookup structures, and domain structures participating in cyclic references across the runtime boundary.

### Rules

- **R-PYO3-001** SHOULD: String representations and mapping accessors in domain models SHOULD utilize PyBackedStr and intern to minimize allocation overhead during repeated key lookups.
- **R-PYO3-002** MANDATORY: The consumer MUST discover and derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-PYO3-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding sequence: find dependency manifest, identify build tool, inspect lock/resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run for version-sensitive behavior at point of use.
- **R-PYO3-004** MANDATORY: Domain models requiring garbage collection integration must implement PyGcTraverse to coordinate with PyVisit callbacks and return PyTraverseError upon traversal failure.

### Verify

```bash
# Discover and run the project test execution script from the repository configuration to execute domain modeling and boundary traversal tests.
# Discover and run the static analysis and linting script declared in repository metadata to verify traversal contract compliance across domain modules.
```

**Accept when:**
- All domain models crossing the runtime boundary implement required traversal protocols without reporting untracked reference leaks.
- All verification scripts pass without type conversion errors or memory safety violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites, code reviews, and traversal checks strictly enforce all rules; violations block merging.
</enforcement>