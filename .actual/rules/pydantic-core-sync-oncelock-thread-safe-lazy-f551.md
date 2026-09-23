# Adoption of std::sync::OnceLock for Thread-Safe Lazy Initialization: Modules Requiring Deferred Thread Safe Initialization

These rules are ALWAYS ACTIVE for all core library modules requiring lazy or one-time initialization of static data structures, thread-safe shared caches, and schema serialization lookup tables.

### Rules

- **R-SYNC-001** MUST: Modules requiring deferred, thread-safe initialization of shared or static data MUST use std::sync::OnceLock to manage cell state.

### Verify

```bash
# Discover and run the project static analysis and linting routines to verify std::sync::OnceLock usage
# Execute the repository test suite with concurrency and race detection flags enabled
```

**Accept when:**
- All static analysis and linting checks pass without warnings regarding unsafe static mutation or non-standard synchronization mechanisms.
- All concurrency test suites complete successfully without detecting data races or deadlocks during lazy initialization.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>