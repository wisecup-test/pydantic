# Worker Service Boundary Parameter Resolution via query_args: Before Implementing Dependencies External Resource Clients

These rules are ALWAYS ACTIVE for worker execution boundaries, runtime communication endpoints, and external resource retrieval scripts parsing input query parameters.

### Rules

- **R-WRK-001** MUST: Before implementing dependencies or external resource clients, the consumer MUST inspect the repository lock artifact to verify the exact resolved version.

### Verify

```bash
# Discover the project dependency manifest and lock artifact, check resolved versions, then run repository test suites and linters.
```

**Accept when:**
- Worker execution entry points successfully parse input query parameters to resolve external resource endpoints.
- Anomalous network responses and parsing exceptions trigger documented error logging routines without crashing the worker context.

<enforcement>
Claude Code MUST NOT skip or defer verification. Code reviews and CI test suites will block changes bypassing parameter validation or omitting error handling at network boundaries.
</enforcement>