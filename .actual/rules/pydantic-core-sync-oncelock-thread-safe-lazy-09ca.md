# Adoption of std::sync::OnceLock for Thread-Safe Lazy Initialization: Before Introducing Updating Dependencies Related Synchronization

These rules are ALWAYS ACTIVE for all core library modules, shared static configurations, format registries, and cached lookup structures requiring lazy or one-time initialization.

### Rules

- **R-SYNC-001** MUST: Before introducing or updating dependencies related to synchronization primitives, engineers MUST inspect the project lock artifact to confirm resolved dependency specifications.
- **R-SYNC-002** MUST: Instantiate std::sync::OnceLock instances in static or long-lived structures and initialize via get_or_init during first runtime request.
- **R-SYNC-003** MUST: Ensure types held inside std::sync::OnceLock fulfill necessary thread-safety marker traits for safe sharing across threads.
- **R-SYNC-004** MUST: Ensure initialization closures do not invoke functions that attempt to read or initialize the same cell instance to prevent re-entrant deadlocks.

### Verify

```bash
# Discover and run the project static analysis and linting routines
cargo check --all-targets
cargo clippy --all-targets -- -D warnings

# Execute the repository test suite with concurrency and race detection flags enabled
cargo test --all-targets -- --test-threads=1
RUSTFLAGS="-Zsanitizer=thread" cargo test --lib
```

**Accept when:**
- All static analysis and linting checks pass without warnings regarding unsafe static mutation or non-standard synchronization mechanisms.
- All concurrency test suites complete successfully without detecting data races or deadlocks during lazy initialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests introducing raw mutable statics or non-standard synchronization mechanisms will be rejected during automated checks and code review.
</enforcement>