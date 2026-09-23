# pyo3 Library Adoption for Rust-Python Interoperability: Before Implementing Compiling Any Component Depending

These rules are ALWAYS ACTIVE for all native data structures, validation modules, and boundary components interfacing directly with foreign runtime objects.

### Rules

- **R-PYO3-001** MUST: Before implementing or compiling any component depending on pyo3, the consumer MUST inspect the repository dependency lock artifact to determine the exact resolved dependency version and verify that all invoked interfaces match that version reference documentation.

### Verify

```bash
# Discover the project build and test runner scripts from the repository configuration and execute the suite
# Example discovery & execution:
if [ -f "Cargo.toml" ]; then
  cargo test
  cargo clippy --all-targets --all-features
fi
```

**Accept when:**
- All test suites verifying foreign-function interface type conversions, exception propagation, and thread-safe initialization pass without errors or panics.
- Repository static analysis and compilation checks succeed across all boundary modules without warnings regarding unhandled foreign runtime exceptions or unmanaged statics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites and peer code review enforce boundary conversion contracts, exception propagation, and lock-file version resolution.
</enforcement>