# Adoption of std::sync::OnceLock for Thread-Safe Lazy Initialization: Consumers Evaluate Read Access Frequency Use

These rules are ALWAYS ACTIVE for all core library modules, shared static configurations, format registries, and cached lookup structures requiring lazy or one-time initialization across thread boundaries.

### Rules

- **R-SYNC-001** SHOULD: Consumers evaluate read access frequency and use get on std::sync::OnceLock instances after initialization to minimize synchronization overhead in performance-critical execution paths.
- **R-SYNC-002** MANDATORY: Instantiate std::sync::OnceLock instances in static or long-lived structures and initialize via get_or_init during first runtime request.
- **R-SYNC-003** MANDATORY: Ensure types held inside std::sync::OnceLock fulfill necessary thread-safety marker traits for safe sharing across threads.
- **R-SYNC-004** MANDATORY: Enforce strict boundaries ensuring initialization closures do not invoke functions that attempt to read or initialize the same cell instance to prevent deadlocks.

### Verify

```bash
# Discover and run the project static analysis and linting routines to verify std::sync::OnceLock usage
cargo check --all-targets
cargo clippy --all-targets -- -D warnings

# Execute the repository test suite with concurrency and race detection flags enabled
cargo test --all-targets -- --test-threads=4
```

**Accept when:**
- All static analysis and linting checks pass without warnings regarding unsafe static mutation or non-standard synchronization mechanisms.
- All concurrency test suites complete successfully without detecting data races or deadlocks during lazy initialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks in the continuous integration pipeline and peer code review verification of synchronization primitives and closure safety.
</enforcement>