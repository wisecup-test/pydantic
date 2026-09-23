# micropip Dynamic Package Installation in WebAssembly Environments: Before Declaring Dependencies Runtime Installation Consumer

These rules are ALWAYS ACTIVE for test harness runners, validation scripts operating within WebAssembly runtime boundaries, and in-browser or sandboxed validation suites that execute compiled wheels directly.

### Rules

- **R-WASM-001** MUST: Before declaring dependencies for in-runtime installation, the consumer MUST inspect the repository lock artifact to determine exact resolved versions.

### Verify

```bash
# Discover the project's WebAssembly test runner script and execute it within the configured runtime container
# Inspect the test execution logs to verify that all in-runtime dependency installations complete successfully before test execution starts
```

**Accept when:**
- The runtime test runner completes with exit code zero and all test assertions pass in the sandboxed environment.
- Dynamic dependency resolution finishes without unhandled network or extraction exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via automated continuous integration checks and peer code review.
</enforcement>