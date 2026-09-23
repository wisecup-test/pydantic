# PyO3 Garbage Collection Traversal Protocol Implementation: Before Implementing Updating Traversal Integrations Developers

These rules are ALWAYS ACTIVE for all native extension structures within serialization, validation, and tool modules that store or manage Python object references, and Rust types participating in Python garbage-collected object graphs requiring cyclic reference detection.

### Rules

- **R-PYO3-001** MUST: Before implementing or updating traversal integrations, developers MUST inspect the repository dependency manifest and authoritative lock artifact to resolve the exact locked version of the pyo3 library and its documentation.
- **R-PYO3-002** MUST: When defining native types that hold Python object references or callable serializers, implement the visitor trait by iterating over each encapsulated Python reference and passing it to the visitor callback.
- **R-PYO3-003** MUST: Ensure error propagation is handled immediately upon visitor callback failure by returning the resulting traverse error to the calling runtime.

### Verify

```bash
# Inspect the project dependency manifest and lock artifact to confirm the declared pyo3 dependency version and traversal feature configuration.
# Execute the repository test suite via the project build tool to verify that cyclic garbage collection tests pass and no memory leaks are detected.
# Run project static analysis checks to verify that every native structure retaining Python runtime objects provides a traversal visitor implementation.
```

**Accept when:**
- All native structures holding Python runtime references implement visitor traversal methods returning pyo3::PyTraverseError on failure.
- Automated cyclic garbage collection and reference cycle test suites pass without memory leaks.
- The resolved pyo3 library version is validated against the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>