# Standardized Schema Error Construction via crate::build_tools::py_schema_err: Developers Discover Project Dependency Manifest Lock

These rules are ALWAYS ACTIVE for schema parsing and configuration decoding across validator builders, serializer initialization, and type serializer builder routines.

### Rules

- **R-SCHEMA-001** MUST: Developers MUST discover the project dependency manifest and lock artifact to inspect the exact resolved dependency versions before implementing or modifying interfaces interacting with external foreign-function interface bindings.
- **R-SCHEMA-002** MUST: Use `crate::build_tools::py_schema_err` across all validator and serializer builders to ensure consistent schema error creation and uniform diagnostic formatting.
- **R-SCHEMA-003** MUST: Isolate schema compilation errors from runtime validation errors to separate schema configuration defects from operational input validation failures.

### Verify

```bash
# Discover and execute repository compilation, tests, and linter checks
cargo check --all-targets
cargo test
cargo clippy --all-targets -- -D warnings
```

**Accept when:**
- All schema compilation failures across validators and serializers yield the expected schema error exception.
- Project compilation and automated test suites pass without unresolved error construction mismatches.
- Code review confirms new and modified builders employ `crate::build_tools::py_schema_err` for invalid schema scenarios.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>