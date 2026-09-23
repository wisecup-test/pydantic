# Standardization on pyo3::IntoPyObjectExt for Rust-to-Python Object Conversion: Modules Exposing Native Internal Validation Serialization

These rules are ALWAYS ACTIVE for modules exposing native internal validation or serialization structures across the foreign function interface boundary.

### Rules

- **R-INT-001** MUST: Modules exposing native internal validation or serialization structures across the foreign function interface boundary MUST convert values into foreign runtime representations using the pyo3::IntoPyObjectExt trait.

### Verify

```bash
# Discover and execute the workspace verification script in the repository configuration
cargo check --all-targets
cargo test
```

**Accept when:**
- All validator and serializer conversion modules compile without trait mismatch errors or deprecation warnings.
- Test suites verifying foreign function interface object conversion pass with zero regressions in memory safety or conversion accuracy.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and mandatory peer code reviews validating that new validators and serializers implement pyo3::IntoPyObjectExt.
</enforcement>