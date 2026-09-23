# pyo3 Library Adoption for Rust-Python Interoperability: Native Internal Data Structures Not Expose

These rules are ALWAYS ACTIVE for native data structures and validation modules interfacing directly with foreign runtime objects, including components implementing boundary type conversions, static string interning, or runtime exception translation.

### Rules

- **R-PYO3-001** MUST_NOT: Native internal data structures MUST_NOT expose raw foreign runtime pointers directly outside the designated foreign-function interface boundary modules.

### Verify

```bash
# Discover and run project build and test runner scripts to verify compilation and contract conformance of all FFI modules.
# Discover and run linting and formatting verification tasks defined in the workspace configuration.
```

**Accept when:**
- All test suites verifying foreign-function interface type conversions, exception propagation, and thread-safe initialization pass without errors or panics.
- Repository static analysis and compilation checks succeed across all boundary modules without warnings regarding unhandled foreign runtime exceptions or unmanaged statics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites and peer code reviews enforce compliance.
</enforcement>