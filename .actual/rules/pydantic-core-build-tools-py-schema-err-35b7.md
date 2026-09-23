# Standardized Schema Error Construction via crate::build_tools::py_schema_err: Schema Dictionary Inspection Within Builder Routines

These rules are ALWAYS ACTIVE for all schema parsing, configuration decoding, validator builders, and type serializer builder routines across the core serialization and validation subsystems.

### Rules

- **R-SCHEMA-001** SHOULD: Schema dictionary inspection within builder routines SHOULD use interned string representations via pyo3::intern when querying frequently accessed configuration keys.
- **R-SCHEMA-002** MANDATORY: When adding new validators or serializers, import crate::build_tools::py_schema_err to handle missing, invalid, or mutually exclusive schema options, and combine it with pyo3::intern for key lookup.

### Verify

```bash
# Discover and execute the repository compilation command
# (e.g., cargo build)

# Discover and run the project test suite covering schema compilation and invalid schema error emission
# (e.g., cargo test)

# Discover and execute the project linter to check that no direct unformatted exception constructors bypass the shared build tools helper
# (e.g., cargo clippy)
```

**Accept when:**
- All schema compilation failures across validators and serializers yield the expected schema error exception.
- Project compilation and automated test suites pass without unresolved error construction mismatches.
- Code review confirms new and modified builders employ crate::build_tools::py_schema_err for invalid schema scenarios.

<enforcement>
Verified by: Continuous integration test suites verifying schema error emissions, static type analysis, and peer code review. Pull requests utilizing direct exception creation or runtime validation error types for schema errors must be blocked until refactored to use crate::build_tools::py_schema_err.
</enforcement>