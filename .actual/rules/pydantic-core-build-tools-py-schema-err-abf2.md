# Standardized Schema Error Construction via crate::build_tools::py_schema_err: Schema Building Configuration Parsing Validator Serializer

These rules are ALWAYS ACTIVE for all schema building, configuration parsing, validator construction, and serializer initialization code routines.

### Rules

- **R-SCHEMA-ERR-001** MUST: Schema building, configuration parsing, and validator or serializer construction routines MUST use crate::build_tools::py_schema_err to construct and return errors whenever an invalid schema definition or unsupported configuration is detected.

### Verify

```bash
# Discover and execute the repository compilation command to ensure all schema builders adhere to error type contracts
# Discover and run the project test suite covering schema compilation and invalid schema error emission
# Discover and execute the project linter to check that no direct unformatted exception constructors bypass the shared build tools helper
```

**Accept when:**
- All schema compilation failures across validators and serializers yield the expected schema error exception.
- Project compilation and automated test suites pass without unresolved error construction mismatches.
- Code review confirms new and modified builders employ crate::build_tools::py_schema_err for invalid schema scenarios.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test suites, static type analysis, and peer code review.
</enforcement>