# Standardization on pyo3::IntoPyObjectExt for Rust-to-Python Object Conversion: Implementations Not Bypass Pyo3 Intopyobjectext Constructing

These rules are ALWAYS ACTIVE for native core validator routines, serializer definitions, input conversion, and lookup key modules interfacing with foreign mappings and dictionary objects.

### Rules

- **R-INTO-001** MUST_NOT: Implementations MUST NOT bypass pyo3::IntoPyObjectExt by constructing unmanaged raw foreign pointers or invoking deprecated conversion traits for values crossing into the foreign runtime.

### Verify

```bash
# Discover the workspace verification script in the repository configuration and execute static analysis:
cargo check --all-targets
cargo test
```

**Accept when:**
- All validator and serializer conversion modules compile without trait mismatch errors or deprecation warnings.
- Test suites verifying foreign function interface object conversion pass with zero regressions in memory safety or conversion accuracy.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing workspace compilation and test suites, and mandatory peer code reviews.
</enforcement>