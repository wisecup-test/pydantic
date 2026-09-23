# Worker Service Boundary Parameter Resolution via query_args: Unexpected Network Responses Execution Errors Worker

These rules are ALWAYS ACTIVE for worker execution boundaries that parse input query parameters to coordinate external resource retrieval and runtime communication endpoints dispatching remote network fetch calls.

### Rules

- **R-WORKER-001** MUST: Unexpected network responses and execution errors at the worker boundary MUST be captured and reported via console.error logging.

### Verify

```bash
# Discover the repository test runner from the root configuration and execute worker integration test suites.
# Discover and run the project code linter and type checker across worker scripts to verify query parameter handling and error branch coverage.
```

**Accept when:**
- Worker execution entry points successfully parse input query parameters to resolve external resource endpoints.
- Anomalous network responses and parsing exceptions trigger documented error logging routines without crashing the worker context.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>