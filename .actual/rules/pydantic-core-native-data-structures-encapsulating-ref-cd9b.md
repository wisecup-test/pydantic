# PyO3 Garbage Collection Traversal Protocol Implementation: Native Data Structures Encapsulating Referencing Python

These rules are ALWAYS ACTIVE for native extension structures within serialization, validation, and tool modules that store or manage Python object references, and Rust types participating in Python garbage-collected object graphs requiring cyclic reference detection.

### Rules

- **R-PYO3-001** MUST: Native data structures encapsulating or referencing Python runtime objects within serialization, validation, or utility modules MUST implement the pyo3 garbage collection traversal protocol utilizing pyo3::PyVisit and pyo3::PyTraverseError.

### Verify

```bash
# Inspect project dependency manifest and lock artifact to confirm pyo3 dependency version and traversal feature configuration
# Execute repository test suite via the project build tool to verify cyclic garbage collection tests pass and no memory leaks are detected
# Run project static analysis checks to verify every native structure retaining Python runtime objects provides a traversal visitor implementation
```

**Accept when:**
- All native structures holding Python runtime references implement visitor traversal methods returning pyo3::PyTraverseError on failure.
- Automated cyclic garbage collection and reference cycle test suites pass without memory leaks.
- The resolved pyo3 library version is validated against the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration test runs, code review, and static analysis checks.
</enforcement>