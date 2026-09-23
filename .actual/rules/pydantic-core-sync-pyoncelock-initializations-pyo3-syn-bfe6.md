# Thread-Safe Lazy Initialization of Python Runtime Objects via pyo3::sync::PyOnceLock: Initializations Pyo3 Sync Pyoncelock Acquire Foreign

These rules are ALWAYS ACTIVE for native extension modules that require repeated access to foreign runtime types, exception classes, singletons, or lookup structures across threads, and validation, serialization, input handling, and error formatting routines interacting with the foreign language runtime.

### Rules

- **R-SYNC-001** MUST: Initializations using pyo3::sync::PyOnceLock MUST acquire the foreign runtime interpreter context through standard acquisition mechanisms before evaluating the initialization closure.

### Verify

```bash
# Discover workspace configuration manifest and execute test runner
# Inspect native source files to confirm static foreign runtime objects use pyo3::sync::PyOnceLock
cargo test
```

**Accept when:**
- All native test suites execute cleanly with zero thread contention errors or deadlocks during concurrent execution.
- Static foreign runtime object access across core modules is mediated by pyo3::sync::PyOnceLock initialization routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration verification suites and peer code review enforce these invariants.
</enforcement>