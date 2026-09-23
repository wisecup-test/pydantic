# PyO3 Garbage Collection Traversal Protocol Implementation: Native Traversal Implementations Propagate Visitor Callback

These rules are ALWAYS ACTIVE for native extension structures within serialization, validation, and tool modules that store or manage Python object references and participate in Python garbage-collected object graphs.

### Rules

- **R-PYO3-001** MUST: Native traversal implementations MUST propagate visitor callback errors immediately by returning pyo3::PyTraverseError to the calling runtime traversal routine.

### Verify

```bash
# Inspect project dependency manifest and lock artifact to confirm the declared pyo3 dependency version and traversal feature configuration
# Execute repository test suite via the project build tool to verify cyclic garbage collection tests pass and no memory leaks are detected
# Run static analysis checks to verify every native structure retaining Python runtime objects provides a traversal visitor implementation
```

**Accept when:**
- All native structures holding Python runtime references implement visitor traversal methods returning pyo3::PyTraverseError on failure.
- Automated cyclic garbage collection and reference cycle test suites pass without memory leaks.
- The resolved pyo3 library version is validated against the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification via continuous integration test suites, code reviews, and static analysis/compiler checks is mandatory.
</enforcement>