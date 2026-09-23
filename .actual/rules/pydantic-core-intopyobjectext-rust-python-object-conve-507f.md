# Standardization on pyo3::IntoPyObjectExt for Rust-to-Python Object Conversion: Custom Validator Contracts Return Fallible Infallible

These rules are ALWAYS ACTIVE for native core validator routines, serializer definitions, input conversion, and lookup key modules interfacing with foreign mappings and dictionary objects.

### Rules

- **R-PYO3-001** MAY: Custom validator contracts MAY return Fallible or Infallible conversion representations when mapping intermediate native errors directly into foreign exception types.

### Verify

\`\`\`bash
# Discover workspace verification script in repository configuration and execute static analysis/test suite
cargo test
cargo clippy --all-targets --all-features
\`\`\`

**Accept when:**
- All validator and serializer conversion modules compile without trait mismatch errors or deprecation warnings.
- Test suites verifying foreign function interface object conversion pass with zero regressions in memory safety or conversion accuracy.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and mandatory peer code reviews.
</enforcement>