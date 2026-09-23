# PyO3 PyBackedStr Adoption for Zero-Copy String References in Core FFI Pipelines: Before Implementing Updating Bindings Relying External

These rules are ALWAYS ACTIVE for all components implementing key lookup, field extraction, argument parsing, or type serialization across the foreign function interface boundary, and internal data structures retaining string keys, field identifiers, or schema definition references derived from foreign runtime objects.

### Rules

- **R-ADR-001** MUST: Before implementing or updating bindings relying on external dependency APIs, consumers MUST discover the project dependency manifest and lock artifact to determine and verify the exact resolved version in the project documentation.
- **R-ADR-002** MUST: When constructing lookup keys and field mappings, convert foreign strings into pyo3::pybacked::PyBackedStr at the earliest boundary entry point to prevent intermediate allocations.
- **R-ADR-003** SHOULD: Combine pyo3::pybacked::PyBackedStr with shared reference wrappers where serializers or validators need to share key definitions across sub-validators.

### Verify

```bash
# Discover and execute the project compilation test suite to verify foreign function interface type compatibility and lifetime constraints
# Discover and run the project memory and performance benchmark suite to ensure absence of redundant string allocation regressions
# Execute the project automated static analysis and linting verification routines to detect unintended conversions to owned heap strings
```

**Accept when:**
- All compilation and test verification suites pass with zero lifetime errors or type mismatches across foreign function interface boundaries.
- Static analysis checks confirm that target string lookups and serialization fields utilize pyo3::pybacked::PyBackedStr without unexpected allocations.
- Performance benchmarks demonstrate the absence of allocation regressions in hot-path validation and serialization scenarios.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis, peer code review, and automated benchmark regression suites enforce compliance.
</enforcement>