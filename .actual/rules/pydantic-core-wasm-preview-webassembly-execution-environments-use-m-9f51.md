# micropip Dynamic Package Installation in WebAssembly Environments: Webassembly Execution Environments Use Micropip Install

These rules are ALWAYS ACTIVE for test harness runners and validation scripts operating within WebAssembly runtime boundaries and sandboxed validation suites executing compiled wheels directly.

### Rules

- **R-MIC-001** MUST: WebAssembly execution environments MUST use micropip.install to resolve and mount dynamic dependencies and wheel artifacts directly into the active runtime context.

### Verify

```bash
# Discover the project's WebAssembly test runner script and execute it within the configured runtime container.
# Inspect the test execution logs to verify that all in-runtime dependency installations complete successfully before test execution starts.
```

**Accept when:**
- The runtime test runner completes with exit code zero and all test assertions pass in the sandboxed environment.
- Dynamic dependency resolution finishes without unhandled network or extraction exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>