# Worker Service Boundary Parameter Resolution via query_args: Worker Service Boundary Definitions Access Configuration

These rules are ALWAYS ACTIVE for worker execution boundaries that parse input query parameters to coordinate external resource retrieval and runtime communication endpoints dispatching remote network fetch calls based on parsed version identifiers.

### Rules

- **R-WRK-001** MUST: Worker service boundary definitions MUST access configuration parameters through query_args lookups when resolving target resource versions before initiating remote network requests.

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