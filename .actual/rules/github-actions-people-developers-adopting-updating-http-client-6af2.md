# Adoption of Requests for External Integration Invocations: Developers Adopting Updating Http Client Library

These rules are ALWAYS ACTIVE for automated integration routines, workflow scripts executing external network calls, and synchronous external API client invocations requiring structured JSON query payloads.

### Rules

- **R-HTTP-001** MUST: Developers adopting or updating the HTTP client library MUST inspect the repository lock artifact to determine the exact resolved dependency version prior to implementation.

### Verify

```bash
# Discover the test runner from the project repository configuration and execute the integration test suite.
# Discover the repository static analysis suite and verify that network client calls configure timeout parameters.
```

**Accept when:**
- Integration suites execute outbound network operations using the standard client with timeout parameters configured.
- Verification processes confirm all external API interaction handlers evaluate status codes and log diagnostic outputs on error.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>