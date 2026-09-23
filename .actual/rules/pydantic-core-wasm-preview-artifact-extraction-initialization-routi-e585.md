# micropip Dynamic Package Installation in WebAssembly Environments: Artifact Extraction Initialization Routines Verify Payload

These rules are ALWAYS ACTIVE for test harness runners, validation scripts, and sandboxed validation suites operating within WebAssembly runtime boundaries.

### Rules

- **R-MIC-001** SHOULD: Artifact extraction and initialization routines SHOULD verify payload integrity and structure prior to initiating test runner entry points.

### Verify

```bash
# Discover the project's WebAssembly test runner script and execute it within the configured runtime container
# Inspect test execution logs to verify that all in-runtime dependency installations complete successfully
```

**Accept when:**
- The runtime test runner completes with exit code zero and all test assertions pass in the sandboxed environment.
- Dynamic dependency resolution finishes without unhandled network or extraction exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and peer code review.
</enforcement>