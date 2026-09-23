# Adoption of Requests for External Integration Invocations: Calls External Endpoints Requests Post Specify

These rules are ALWAYS ACTIVE for automated integration routines and workflow scripts executing external network calls.

### Rules

- **R-EXT-001** MUST: Calls to external endpoints using requests.post MUST specify authorization headers and structured JSON request bodies.

### Verify

```bash
# Discover the test runner from the project repository configuration and execute the integration test suite.
# Discover the repository static analysis suite and verify that network client calls configure timeout parameters.
```

**Accept when:**
- Integration suites execute outbound network operations using the standard client with timeout parameters configured.
- Verification processes confirm all external API interaction handlers evaluate status codes and log diagnostic outputs on error.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites and peer code reviews.
</enforcement>