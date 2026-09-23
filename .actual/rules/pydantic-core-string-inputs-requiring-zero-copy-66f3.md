# PyO3 Adoption for Host Exception Mapping and Native Boundary Interoperability: String Inputs Requiring Zero Copy Extraction

These rules are ALWAYS ACTIVE for native validation routines, serialization/deserialization handlers, and error conversion layers interfacing with host interpreter objects across the foreign function interface boundary.

### Rules

- **R-PYO3-001** SHOULD: String inputs requiring zero-copy extraction across the boundary SHOULD utilize `pyo3::pybacked::PyBackedStr` where lifetime preservation without copying is supported.
- **R-PYO3-002** MANDATORY: Ensure all type mismatch paths in validation and serialization routines instantiate `pyo3::exceptions::PyTypeError` via established helper functions to preserve structured error details.
- **R-PYO3-003** MANDATORY: Employ `pyo3::sync::PyOnceLock` for singleton constructs requiring lazy evaluation under the host interpreter lifecycle.

### Verify

```bash
# Discover and run the project test suite via the repository build tool to verify exception propagation and boundary conversions.
# Discover and execute the repository static analysis and linting scripts to verify compliance with binding conventions and safety constraints.
```

**Accept when:**
- All boundary validation and serialization tests pass with invalid input types correctly surfacing as host type errors.
- No unhandled native panic occurs across any foreign function interface boundary.
- Static verification and repository linting workflows complete without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration testing, peer code reviews, and repository static analysis checks. Violations block merging.
</enforcement>