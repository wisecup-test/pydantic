# PyO3 Adoption for Host Exception Mapping and Native Boundary Interoperability: Core Validation Serialization Modules Interface Host

These rules are ALWAYS ACTIVE for all core validation, serialization, and error conversion modules interacting with the host runtime via PyO3.

### Rules

- **R-PYO3-001** MUST: Core validation and serialization modules MUST interface with the host runtime using pyo3 primitives and raise pyo3::exceptions::PyTypeError for input type mismatch failures.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration testing and peer code reviews verify enforcement.
</enforcement>