# Standardized Schema Error Construction via crate::build_tools::py_schema_err: Builder Routines Not Construct Hoc Exception

These rules are ALWAYS ACTIVE for all schema parsing, configuration decoding, validator builders, and serializer initialization routines across the project.

### Rules

- **R-SCHEMA-001** MUST_NOT: Builder routines MUST NOT construct ad-hoc exception instances or return runtime validation error types for errors occurring during schema compilation and configuration parsing.

### Verify

```bash
# Discover and execute the repository compilation command
# Discover and run the project test suite covering schema compilation and invalid schema error emission
# Discover and execute the project linter to check that no direct unformatted exception constructors bypass the shared build tools helper
```

**Accept when:**
- All schema compilation failures across validators and serializers yield the expected schema error exception.
- Project compilation and automated test suites pass without unresolved error construction mismatches.
- Code review confirms new and modified builders employ crate::build_tools::py_schema_err for invalid schema scenarios.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via CI test suites, static type analysis, compiler checks, and peer code review.
</enforcement>