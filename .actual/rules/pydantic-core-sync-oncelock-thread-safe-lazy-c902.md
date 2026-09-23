# Adoption of std::sync::OnceLock for Thread-Safe Lazy Initialization: Modules Not Use Mutable Static Variables

These rules are ALWAYS ACTIVE for all core library modules and shared static configurations requiring lazy or one-time initialization.

### Rules

- **R-SYNC-001** MUST_NOT: Modules MUST NOT use mutable static variables or unsafe memory blocks to achieve lazy or once-only global initialization.
- **R-SYNC-002** MUST: Instantiate std::sync::OnceLock instances in static or long-lived structures and initialize via get_or_init during first runtime request.
- **R-SYNC-003** MUST: Ensure types held inside std::sync::OnceLock fulfill necessary thread-safety marker traits for safe sharing across threads.
- **R-SYNC-004** MUST: Keep initialization logic lightweight and defer heavy non-essential computations outside the one-time cell initialization block.
- **R-SYNC-005** MUST: Ensure initialization closures do not invoke functions that attempt to read or initialise the same cell instance to prevent re-entrant deadlocks.

### Verify

```bash
# Discover and run project static analysis and linting routines
# (Command must be derived from the project repository configuration, e.g., cargo clippy)
cargo clippy --all-targets --all-features

# Execute repository test suite with concurrency and race detection flags enabled
cargo test --all-targets --all-features
```

**Accept when:**
- All static analysis and linting checks pass without warnings regarding unsafe static mutation or non-standard synchronization mechanisms.
- All concurrency test suites complete successfully without detecting data races or deadlocks during lazy initialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks in the continuous integration pipeline and peer code review verification of synchronization primitives and closure safety.
</enforcement>