# Thread-Safe Lazy Initialization of Python Runtime Objects via pyo3::sync::PyOnceLock: Native Modules Not Perform Repeated Dynamic

These rules are ALWAYS ACTIVE for all native extension modules that require repeated access to foreign runtime types, exception classes, singletons, or lookup structures across threads, and validation, serialization, input handling, and error formatting routines interacting with the foreign language runtime.

### Rules

- **R-SYNC-001** MUST_NOT: Native modules MUST NOT perform repeated dynamic module imports or type resolutions within hot validation or serialization paths when the target type can be cached via pyo3::sync::PyOnceLock.

### Verify

```bash
# Discover workspace configuration manifest and execute test runner
cargo test
```

**Accept when:**
- All native test suites execute cleanly with zero thread contention errors or deadlocks during concurrent execution.
- Static foreign runtime object access across core modules is mediated by pyo3::sync::PyOnceLock initialization routines.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>