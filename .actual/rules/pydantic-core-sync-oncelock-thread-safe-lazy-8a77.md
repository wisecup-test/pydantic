# Adoption of std::sync::OnceLock for Thread-Safe Lazy Initialization: Initialization Values Managed Std Sync Oncelock

These rules are ALWAYS ACTIVE for all core library modules requiring lazy or one-time initialization of static data structures, thread-safe shared caches, and schema serialization lookup tables.

### Rules

- **R-ONCELOCK-001** MUST: Initialization of values managed by std::sync::OnceLock MUST occur via get_or_init or get_or_try_init to guarantee single-execution semantics across concurrent threads.
- **R-ONCELOCK-002** MUST: Instantiate std::sync::OnceLock instances in static or long-lived structures and initialize via get_or_init during first runtime request.
- **R-ONCELOCK-003** MUST: Ensure types held inside std::sync::OnceLock fulfill necessary thread-safety marker traits for safe sharing across threads.
- **R-ONCELOCK-004** MUST: Keep initialization logic lightweight and defer heavy non-essential computations outside the one-time cell initialization block to avoid blocking contention under high concurrent load.
- **R-ONCELOCK-005** MUST: Enforce strict boundaries ensuring initialization closures do not invoke functions that attempt to read or initialize the same cell instance, preventing re-entrant deadlocks.

### Verify

```bash
# Discover and run the project static analysis and linting routines
cargo clippy --all-targets --all-features -- -D warnings

# Execute the repository test suite with concurrency and race detection flags enabled
cargo test --all-targets --all-features
```

**Accept when:**
- All static analysis and linting checks pass without warnings regarding unsafe static mutation or non-standard synchronization mechanisms.
- All concurrency test suites complete successfully without detecting data races or deadlocks during lazy initialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated static analysis and peer code review of synchronization primitives is mandatory. Pull requests introducing raw mutable statics or non-standard synchronization mechanisms will be rejected.
</enforcement>