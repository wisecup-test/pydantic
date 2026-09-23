# PyO3 Adoption for Host Exception Mapping and Native Boundary Interoperability: Shared Static State Lazily Evaluated Host

These rules are ALWAYS ACTIVE for all native extension modules, validation routines, serialization handlers, and error conversion layers interfacing with the host interpreter runtime.

### Rules

- **R-PYO3-001** SHOULD: Shared static state and lazily evaluated host references SHOULD use `pyo3::sync::PyOnceLock` to ensure thread-safe initialization under host runtime threading constraints.
- **R-PYO3-002** MUST: Ensure all type mismatch paths in validation and serialization routines instantiate `pyo3::exceptions::PyTypeError` via established helper functions to preserve structured error details.
- **R-PYO3-003** MUST: Employ `pyo3::sync::PyOnceLock` for singleton constructs requiring lazy evaluation under the host interpreter lifecycle.

### Verify

\`\`\`bash
# Discover and run the project test suite via the repository build tool to verify exception propagation and boundary conversions.
# Discover and execute the repository static analysis and linting scripts to verify compliance with binding conventions and safety constraints.
cargo test
cargo clippy
\`\`\`

**Accept when:**
- All boundary validation and serialization tests pass with invalid input types correctly surfacing as host type errors.
- No unhandled native panic occurs across any foreign function interface boundary.
- Static verification and repository linting workflows complete without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration testing, peer code review, and repository static analysis.
</enforcement>