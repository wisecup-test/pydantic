# PyO3 Adoption for Host Exception Mapping and Native Boundary Interoperability: Native Boundary Routines Not Construct Arbitrary

These rules are ALWAYS ACTIVE for all native validation routines, serialization/deserialization handlers, and error conversion layers interfacing directly with host interpreter objects.

### Rules

- **R-PYO3-001** MUST_NOT: Native boundary routines MUST_NOT construct arbitrary unchecked native error variants when host callers expect standard type failure contracts represented by pyo3::exceptions::PyTypeError.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration testing, peer code review, and static analysis checks.
</enforcement>