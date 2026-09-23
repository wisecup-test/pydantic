# Standardization on pyo3::IntoPyObjectExt for Rust-to-Python Object Conversion: Shared Validator Serializer Configurations Crossing Thread

These rules are ALWAYS ACTIVE for all native core validator routines, serializer definitions, input conversion, and lookup key modules interfacing with foreign mappings and dictionary objects crossing thread or invocation boundaries.

### Rules

- **R-PYO3-001** SHOULD: Shared validator or serializer configurations crossing thread or invocation boundaries SHOULD wrap instances in std::sync::Arc and utilize std::borrow::Cow where clone-on-write semantics prevent unnecessary memory duplication.

### Verify

```bash
# Discover workspace verification script in repository configuration and execute static analysis suite
# Run project test suite across all validator and serializer test fixtures
```

**Accept when:**
- All validator and serializer conversion modules compile without trait mismatch errors or deprecation warnings.
- Test suites verifying foreign function interface object conversion pass with zero regressions in memory safety or conversion accuracy.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and mandatory peer code reviews.
</enforcement>