# Standardized Schema Error Construction via crate::build_tools::py_schema_err: Errors Constructed Via Crate Build Tools

These rules are ALWAYS ACTIVE for all schema parsing, configuration decoding, validator builders, and serializer initialization routines across the codebase.

### Rules

- **R-BUILD-001** MUST: Errors constructed via crate::build_tools::py_schema_err MUST propagate as failed result types that map directly to the standardized schema error exception in the host runtime.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test suites, static type analysis, and peer code review.
</enforcement>