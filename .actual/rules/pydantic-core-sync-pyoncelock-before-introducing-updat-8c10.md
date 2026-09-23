# Thread-Safe Lazy Initialization of Python Runtime Objects via pyo3::sync::PyOnceLock: Before Introducing Updating Usage Pyo3 Sync

These rules are ALWAYS ACTIVE for native extension modules that require repeated access to foreign runtime types, exception classes, singletons, or lookup structures across threads, and validation, serialization, input handling, and error formatting routines interacting with the foreign language runtime.

### Rules

- **R-SYNC-001** MUST: Before introducing or updating usage of pyo3::sync::PyOnceLock, developers MUST inspect the repository dependency lock artifact to verify the exact resolved version of the foreign interface dependency against official documentation.
- **R-SYNC-002** MUST: Encapsulate static pyo3::sync::PyOnceLock instances behind accessor functions that return borrowed references to the cached runtime object.
- **R-SYNC-003** MUST: Keep initialization closures minimal and self-contained to avoid triggering nested calls that could cause deadlock.

### Verify

```bash
# Discover the workspace configuration manifest and execute the test runner to validate synchronization invariants.
# Inspect native source files to confirm that static foreign runtime objects use pyo3::sync::PyOnceLock instead of unsynchronized static state.
```

**Accept when:**
- All native test suites execute cleanly with zero thread contention errors or deadlocks during concurrent execution.
- Static foreign runtime object access across core modules is mediated by pyo3::sync::PyOnceLock initialization routines.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>