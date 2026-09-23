# Standardization on pyo3::IntoPyObjectExt for Rust-to-Python Object Conversion: String Literal Lookups Static Dictionary Key

These rules are ALWAYS ACTIVE for all core data validation, serialization, and input parsing modules interfacing with foreign runtime environments.

### Rules

- **R-PYO3-001** SHOULD: String literal lookups and static dictionary key conversions across the foreign function interface boundary SHOULD coordinate with pyo3::intern or PyBackedStr to avoid duplicate object allocations.
- **R-PYO3-002** MANDATORY: The consumer MUST discover build tools, manifests, and version numbers from the project repository (Lock-version grounding policy).
- **R-PYO3-003** MANDATORY: Internal items for complex collections such as mappings or sequences must be assembled before invoking conversion routines to minimize runtime interpreter lock contention.

### Verify

```bash
# Discover the workspace verification script in the repository configuration and execute the static analysis suite
# Run the project test suite across all validator and serializer test fixtures
cargo test
```

**Accept when:**
- All validator and serializer conversion modules compile without trait mismatch errors or deprecation warnings.
- Test suites verifying foreign function interface object conversion pass with zero regressions in memory safety or conversion accuracy.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing raw foreign pointer manipulation or unapproved conversion mechanisms are automatically blocked by continuous integration.
</enforcement>