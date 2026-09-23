# pyo3 Library Adoption for Rust-Python Interoperability: Components Requiring Lazy One Time Static

These rules are ALWAYS ACTIVE for native data structures, validation modules, and boundary type conversion/exception translation components interfacing directly with foreign runtime objects.

### Rules

- **R-PYO3-001** MUST: Components requiring lazy, one-time static initialization of runtime objects MUST use pyo3::sync::PyOnceLock rather than uncoordinated static mutables or foreign synchronization primitives.

### Verify

```bash
# Discover and execute project build, test runner, and linting scripts from repository configuration
```

**Accept when:**
- All test suites verifying foreign-function interface type conversions, exception propagation, and thread-safe initialization pass without errors or panics.
- Repository static analysis and compilation checks succeed across all boundary modules without warnings regarding unhandled foreign runtime exceptions or unmanaged statics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites and peer code reviews verify boundary conversion contracts and exception propagation.
</enforcement>