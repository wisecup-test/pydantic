# pyo3 Library Adoption for Rust-Python Interoperability: Boundary Validation Failures That Represent Value

These rules are ALWAYS ACTIVE for all native data structures and validation modules interfacing directly with foreign runtime objects, as well as components implementing boundary type conversions, static string interning, or runtime exception translation.

### Rules

- **R-PYO3-001** MUST: Boundary validation failures that represent value errors MUST propagate across the foreign-function interface as pyo3::exceptions::PyValueError rather than causing native panic or silent truncation.

### Verify

```bash
# Discover and execute project build, test runner, linting, and formatting verification scripts from repository configuration
```

**Accept when:**
- All test suites verifying foreign-function interface type conversions, exception propagation, and thread-safe initialization pass without errors or panics.
- Repository static analysis and compilation checks succeed across all boundary modules without warnings regarding unhandled foreign runtime exceptions or unmanaged statics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites and peer code reviews strictly enforce these boundary conversion contracts and exception propagation rules.
</enforcement>