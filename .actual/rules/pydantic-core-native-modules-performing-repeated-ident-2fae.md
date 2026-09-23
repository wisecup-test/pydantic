# pyo3 Library Adoption for Rust-Python Interoperability: Native Modules Performing Repeated Identifier Lookups

These rules are ALWAYS ACTIVE for native data structures and validation modules interfacing directly with foreign runtime objects, and components implementing boundary type conversions, static string interning, or runtime exception translation.

### Rules

- **R-ADR-001** SHOULD: Native modules performing repeated identifier lookups or string comparisons against runtime attributes use pyo3::intern to minimize allocation overhead across the foreign-function boundary.

### Verify

```bash
# Discover the project build and test runner scripts from the repository configuration and execute the suite
# Discover the linting and formatting verification tasks defined in the workspace configuration and execute them
cargo test
cargo clippy --all-targets --all-features
```

**Accept when:**
- All test suites verifying foreign-function interface type conversions, exception propagation, and thread-safe initialization pass without errors or panics.
- Repository static analysis and compilation checks succeed across all boundary modules without warnings regarding unhandled foreign runtime exceptions or unmanaged statics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites exercising boundary conversion contracts and exception propagation, alongside peer code review, are mandatory.
</enforcement>