# PyO3 Garbage Collection Traversal Protocol Implementation: Native Serializers Handling Dynamic String References

These rules are ALWAYS ACTIVE for all native extension structures within serialization, validation, and tool modules that store or manage Python object references, and Rust types participating in Python garbage-collected object graphs requiring cyclic reference detection.

### Rules

- **R-PYO3-001** MAY: Native serializers handling dynamic string references MAY utilize std::borrow::Cow to avoid unnecessary memory allocations when interacting with interned Python strings via pyo3::intern.

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
Claude Code MUST NOT skip or defer verification. Verification is enforced via continuous integration test runs, code review verification of all native structs holding Python runtime references, and static analysis/compiler checks verifying implementation of traversal visitor traits.
</enforcement>