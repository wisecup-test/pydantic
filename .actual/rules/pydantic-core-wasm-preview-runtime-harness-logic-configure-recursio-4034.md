# micropip Dynamic Package Installation in WebAssembly Environments: Runtime Harness Logic Configure Recursion Limits

These rules are ALWAYS ACTIVE for test harness runners and validation scripts operating within WebAssembly runtime boundaries and in-browser or sandboxed validation suites executing compiled wheels directly.

### Rules

- **R-ADR-001** MUST: Runtime harness logic MUST configure recursion limits via sys.setrecursionlimit prior to executing heavy test harnesses in sandboxed interpreters.

### Verify

```bash
# Discover the project's WebAssembly test runner script and execute it within the configured runtime container
# Inspect test execution logs to verify in-runtime dependency installations and recursion configuration complete successfully
```

**Accept when:**
- The runtime test runner completes with exit code zero and all test assertions pass in the sandboxed environment.
- Dynamic dependency resolution finishes without unhandled network or extraction exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>