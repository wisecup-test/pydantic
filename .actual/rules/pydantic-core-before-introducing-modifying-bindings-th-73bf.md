# PyO3 Adoption for Host Exception Mapping and Native Boundary Interoperability: Before Introducing Modifying Bindings That Depend

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PYO3-001** MUST: Before introducing or modifying bindings that depend on pyo3, developers MUST discover the dependency manifest and authoritative repository lock artifact to determine and verify the exact resolved version of pyo3 against official documentation.
- **R-PYO3-002** MUST: Ensure all type mismatch paths in validation and serialization routines instantiate pyo3::exceptions::PyTypeError via established helper functions to preserve structured error details.
- **R-PYO3-003** MUST: Employ pyo3::sync::PyOnceLock for singleton constructs requiring lazy evaluation under the host interpreter lifecycle.

### Verify

```bash
# Discover and run the project test suite via the repository build tool
# Discover and execute the repository static analysis and linting scripts
```

**Accept when:**
- All boundary validation and serialization tests pass with invalid input types correctly surfacing as host type errors.
- No unhandled native panic occurs across any foreign function interface boundary.
- Static verification and repository linting workflows complete without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>