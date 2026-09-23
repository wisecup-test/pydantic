# Thread-Safe Lazy Initialization of Python Runtime Objects via pyo3::sync::PyOnceLock: Native Modules Requiring Static Lazily Resolved

These rules are ALWAYS ACTIVE for all native extension modules requiring static or lazily resolved foreign runtime types, singleton sentinels, or shared lookup tables across threads.

### Rules

- **R-SYNC-001** MUST: Native modules requiring static or lazily resolved foreign runtime types, singleton sentinels, or shared lookup tables MUST utilize pyo3::sync::PyOnceLock to ensure thread-safe one-time initialization.

### Verify

```bash
# Discover the workspace configuration manifest and execute the test runner to validate synchronization invariants
cargo test
```

**Accept when:**
- All native test suites execute cleanly with zero thread contention errors or deadlocks during concurrent execution.
- Static foreign runtime object access across core modules is mediated by pyo3::sync::PyOnceLock initialization routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration verification suites test native extension execution across concurrent threads, and peer code review verifies changes to native extension modules.
</enforcement>