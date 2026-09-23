# Thread-Safe Lazy Initialization of Python Runtime Objects via pyo3::sync::PyOnceLock: Cached Values Stored Pyo3 Sync Pyoncelock

These rules are ALWAYS ACTIVE for native extension modules requiring repeated access to foreign runtime types, exception classes, singletons, or lookup structures across threads.

### Rules

- **R-SYNC-001** MUST_NOT: Cached values stored in pyo3::sync::PyOnceLock MUST NOT retain mutable state that could introduce data races across concurrent threads accessing the shared reference.

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