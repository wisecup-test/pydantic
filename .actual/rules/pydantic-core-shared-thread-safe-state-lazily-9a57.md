# PyO3 Garbage Collection Traversal Protocol Implementation: Shared Thread Safe State Lazily Initialized

These rules are ALWAYS ACTIVE for native extension structures within serialization, validation, and tool modules that store or manage Python object references, and Rust types participating in Python garbage-collected object graphs requiring cyclic reference detection.

### Rules

- **R-PYO3-001** SHOULD: Shared thread-safe state and lazily initialized caches across serialization and validation components SHOULD use `std::sync::Arc` and `pyo3::sync::PyOnceLock` to separate concurrency synchronization from garbage collection traversal.
- **R-PYO3-002** MANDATORY: When defining native types that hold Python object references or callable serializers, implement the visitor trait by iterating over each encapsulated Python reference and passing it to the visitor callback.
- **R-PYO3-003** MANDATORY: Ensure error propagation is handled immediately upon visitor callback failure by returning the resulting traverse error to the calling runtime.
- **R-PYO3-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool from the manifest. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up official documentation, changelog, or public API reference for that exact version. (5) Confirm every API, class, or function you will call exists in that exact version's documentation before using it. (6) For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Direct the build system to inspect the project dependency manifest and lock artifact to confirm the declared pyo3 dependency version and traversal feature configuration.
# Execute the repository test suite via the project build tool to verify that cyclic garbage collection tests pass and no memory leaks are detected.
# Run project static analysis checks to verify that every native structure retaining Python runtime objects provides a traversal visitor implementation.
```

**Accept when:**
- All native structures holding Python runtime references implement visitor traversal methods returning `pyo3::PyTraverseError` on failure.
- Automated cyclic garbage collection and reference cycle test suites pass without memory leaks.
- The resolved `pyo3` library version is validated against the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests that introduce native structures holding Python references without pyo3 traversal implementations will be rejected. Test failures indicating cyclic reference memory leaks block merging into the main branch.
</enforcement>