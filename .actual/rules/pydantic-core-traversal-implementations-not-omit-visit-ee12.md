# PyO3 Garbage Collection Traversal Protocol Implementation: Traversal Implementations Not Omit Visitor Calls

These rules are ALWAYS ACTIVE for all native extension structures within serialization, validation, and tool modules that store or manage Python object references, and Rust types participating in Python garbage-collected object graphs requiring cyclic reference detection.

### Rules

- **R-PYO3-001** MUST_NOT: Traversal implementations MUST NOT omit visitor traversal calls for any nested Python runtime references or closures stored within the enclosing native structure.

### Verify

```bash
# Inspect project dependency manifest and lock artifact to confirm resolved pyo3 version and traversal feature config
# (Build tool and manifest path derived from project repository)

# Execute repository test suite to verify cyclic GC tests pass and no memory leaks are detected
# Run static analysis to verify every native structure retaining Python runtime objects provides a traversal visitor implementation
```

**Accept when:**
- All native structures holding Python runtime references implement visitor traversal methods returning pyo3::PyTraverseError on failure.
- Automated cyclic garbage collection and reference cycle test suites pass without memory leaks.
- The resolved pyo3 library version is validated against the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test runs, code reviews, and static analysis checks will reject violations and test failures indicating cyclic reference memory leaks.
</enforcement>