# micropip Dynamic Package Installation in WebAssembly Environments: Test Suites Executing Within Webassembly Environment

These rules are ALWAYS ACTIVE for test harness runners, validation scripts operating within WebAssembly runtime boundaries, and in-browser or sandboxed validation suites that execute compiled wheels directly.

### Rules

- **R-WASM-001** MUST_NOT: Test suites executing within the WebAssembly environment MUST NOT attempt out-of-process isolation or background thread spawning when executing in single-threaded environments.

### Verify

```bash
# Discover the project's WebAssembly test runner script and execute it within the configured runtime container.
# Inspect the test execution logs to verify that all in-runtime dependency installations complete successfully before test execution starts.
```

**Accept when:**
- The runtime test runner completes with exit code zero and all test assertions pass in the sandboxed environment.
- Dynamic dependency resolution finishes without unhandled network or extraction exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests failing the WebAssembly runtime test harness are blocked from merging.
</enforcement>