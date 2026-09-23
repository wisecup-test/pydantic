# Adoption of Requests for External Integration Invocations: External Network Requests Performed During Automated

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EXT-001** MUST: External network requests performed during automated integration workflows MUST utilize the requests library with explicit timeout configurations.

### Verify

```bash
# Discover the test runner from the project repository configuration and execute the integration test suite.
# Discover the repository static analysis suite and verify that network client calls configure timeout parameters.
```

**Accept when:**
- Integration suites execute outbound network operations using the standard client with timeout parameters configured.
- Verification processes confirm all external API interaction handlers evaluate status codes and log diagnostic outputs on error.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>