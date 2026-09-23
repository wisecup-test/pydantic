# PyO3 PyBackedStr Adoption for Zero-Copy String References in Core FFI Pipelines: Foreign Function Interface Boundaries Ensure That

These rules are ALWAYS ACTIVE for components implementing key lookup, field extraction, argument parsing, or type serialization across the foreign function interface boundary, and internal data structures retaining string keys, field identifiers, or schema definition references derived from foreign runtime objects.

### Rules

- **R-FFI-001** MUST: Foreign function interface boundaries MUST ensure that foreign runtime object lifetimes backing pyo3::pybacked::PyBackedStr instances are preserved for the entire duration of the reference usage.

### Verify

```bash
# Discover and execute the project compilation test suite to verify foreign function interface type compatibility and lifetime constraints.
cargo test --all-targets

# Discover and run the project memory and performance benchmark suite to ensure absence of redundant string allocation regressions.
cargo bench

# Execute the project automated static analysis and linting verification routines to detect unintended conversions to owned heap strings.
cargo clippy --all-targets -- -D warnings
```

**Accept when:**
- All compilation and test verification suites pass with zero lifetime errors or type mismatches across foreign function interface boundaries.
- Static analysis checks confirm that target string lookups and serialization fields utilize pyo3::pybacked::PyBackedStr without unexpected allocations.
- Performance benchmarks demonstrate the absence of allocation regressions in hot-path validation and serialization scenarios.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>