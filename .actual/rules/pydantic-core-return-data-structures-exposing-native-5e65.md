# pyo3 Library Adoption for Rust-Python Interoperability: Return Data Structures Exposing Native Representations

These rules are ALWAYS ACTIVE for native data structures and validation modules interfacing directly with foreign runtime objects, and components implementing boundary type conversions, static string interning, or runtime exception translation.

### Rules

- **R-PYO3-001** SHOULD: Return data structures exposing native representations to the foreign runtime SHOULD implement or derive IntoPyObject to guarantee uniform serialization across boundary contracts.

### Verify

```bash
# Discover the project build and test runner scripts from the repository configuration and execute the suite
# Discover the linting and formatting verification tasks defined in the workspace configuration and execute them
```

**Accept when:**
- All test suites verifying foreign-function interface type conversions, exception propagation, and thread-safe initialization pass without errors or panics.
- Repository static analysis and compilation checks succeed across all boundary modules without warnings regarding unhandled foreign runtime exceptions or unmanaged statics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites exercising boundary conversion contracts, peer code review, and compilation/test checks in boundary conversion modules are mandatory.
</enforcement>