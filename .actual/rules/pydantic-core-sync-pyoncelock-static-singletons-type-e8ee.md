# Thread-Safe Lazy Initialization of Python Runtime Objects via pyo3::sync::PyOnceLock: Static Singletons Type References Cached Inside

These rules are ALWAYS ACTIVE for native extension modules requiring repeated access to foreign runtime types, exception classes, singletons, or lookup structures across threads, including validation, serialization, input handling, and error formatting routines interacting with the foreign language runtime.

### Rules

- **R-SYNC-001** SHOULD: Static singletons and type references cached inside pyo3::sync::PyOnceLock SHOULD be encapsulated within dedicated accessor functions that manage lazy initialization transparently.
- **R-SYNC-002** MANDATORY: Encapsulate static pyo3::sync::PyOnceLock instances behind accessor functions that return borrowed references to the cached runtime object.
- **R-SYNC-003** MANDATORY: Keep initialization closures minimal and self-contained to avoid triggering nested calls that could cause deadlock.

### Verify

```bash
# Discover workspace configuration manifest and execute the test runner
# (e.g., cargo test) to validate synchronization invariants.
cargo test

# Inspect native source files to confirm static foreign runtime objects use pyo3::sync::PyOnceLock
grep -rn "PyOnceLock" src/
```

**Accept when:**
- All native test suites execute cleanly with zero thread contention errors or deadlocks during concurrent execution.
- Static foreign runtime object access across core modules is mediated by pyo3::sync::PyOnceLock initialization routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration verification suites and peer code reviews enforce these rules, and pull requests introducing unsynchronized static state or repeated dynamic type lookups in hot paths are rejected.
</enforcement>