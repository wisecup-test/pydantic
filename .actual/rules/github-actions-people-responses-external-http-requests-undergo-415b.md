# Adoption of Requests for External Integration Invocations: Responses External Http Requests Undergo Status

These rules are ALWAYS ACTIVE for automated integration components and workflow scripts executing external network calls.

### Rules

- **R-EXT-001** MUST: Responses from external HTTP requests MUST undergo HTTP status code evaluation and error logging before extracting payload data.
- **R-EXT-002** MUST: All outbound HTTP client calls configure a finite timeout parameter passed to the invocation method.
- **R-EXT-003** MUST: Validate remote HTTP response status codes and log response payload contents when non-successful statuses are encountered.

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