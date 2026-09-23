# PyO3 PyBackedStr Adoption for Zero-Copy String References in Core FFI Pipelines: Data Structures Holding String References Across

These rules are ALWAYS ACTIVE for components implementing key lookup, field extraction, argument parsing, or type serialization across the foreign function interface boundary, and internal data structures retaining string keys, field identifiers, or schema definition references derived from foreign runtime objects.

### Rules

- **R-PYO3-001** MUST_NOT: Data structures holding string references across serialization and validation boundaries MUST NOT convert incoming foreign strings into owned heap strings unless mutation or detachment from foreign runtime lifetimes is strictly necessary.

### Verify

```bash
# Discover and execute the project compilation test suite to verify foreign function interface type compatibility and lifetime constraints.
# Discover and run the project memory and performance benchmark suite to ensure absence of redundant string allocation regressions.
# Execute the project automated static analysis and linting verification routines to detect unintended conversions to owned heap strings.
```

**Accept when:**
- All compilation and test verification suites pass with zero lifetime errors or type mismatches across foreign function interface boundaries.
- Static analysis checks confirm that target string lookups and serialization fields utilize pyo3::pybacked::PyBackedStr without unexpected allocations.
- Performance benchmarks demonstrate the absence of allocation regressions in hot-path validation and serialization scenarios.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis, peer code review, and automated benchmark regression suites enforce compliance.
</enforcement>