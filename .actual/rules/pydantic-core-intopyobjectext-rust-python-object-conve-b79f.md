# Standardization on pyo3::IntoPyObjectExt for Rust-to-Python Object Conversion: Engineers Discover Ecosystem Lock Artifact Verify

These rules are ALWAYS ACTIVE for native core validator routines, serializer definitions, and input conversion modules interfacing with foreign runtimes.

### Rules

- **R-INTOPY-001** MUST: Engineers MUST discover the ecosystem lock artifact and verify the exact resolved version of the foreign function interface library before writing or updating object conversion implementations.

### Verify

```bash
# Discover workspace verification script in repository configuration and execute static analysis/test suite
cargo check --all-targets
cargo test
```

**Accept when:**
- All validator and serializer conversion modules compile without trait mismatch errors or deprecation warnings.
- Test suites verifying foreign function interface object conversion pass with zero regressions in memory safety or conversion accuracy.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing raw foreign pointer manipulation or unapproved conversion mechanisms are automatically blocked.
</enforcement>