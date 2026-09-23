# PyO3 Adoption for Host Exception Mapping and Native Boundary Interoperability: Recurring String Identifiers Schema Keys Passed

These rules are ALWAYS ACTIVE for native extension validation, serialization, and error boundary mapping routines interfacing with the host interpreter runtime.

### Rules

- **R-PYO3-001** MUST: Recurring string identifiers and schema keys passed across the boundary MUST utilize pyo3::intern to avoid duplicate object allocations in the host runtime.

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
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration testing, peer code review, and repository static analysis and binding linter checks executed during merge validation.
</enforcement>