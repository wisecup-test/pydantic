# Adoption of Requests for External Integration Invocations: Integration Workflows Not Perform Unbounded Http

These rules are ALWAYS ACTIVE for automated integration routines, workflow scripts, and synchronous external client invocations requiring structured JSON payloads.

### Rules

- **R-EXT-001** SHOULD NOT: Integration workflows SHOULD NOT perform unbounded HTTP requests without configured request timeout parameters.

### Verify

```bash
# Discover the test runner from the project repository configuration and execute the integration test suite.
# Discover the repository static analysis suite and verify that network client calls configure timeout parameters.
```

**Accept when:**
- Integration suites execute outbound network operations using the standard client with timeout parameters configured.
- Verification processes confirm all external API interaction handlers evaluate status codes and log diagnostic outputs on error.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated test suites and peer code review.
</enforcement>