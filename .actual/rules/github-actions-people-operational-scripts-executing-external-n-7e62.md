# Standard Library logging Adoption for Operational Script Diagnostics: Operational Scripts Executing External Network Calls

These rules are ALWAYS ACTIVE for operational scripts and automation tasks performing external HTTP requests and configuration loading.

### Rules

- **R-OP-001** MUST: Operational scripts executing external network calls MUST record non-200 HTTP responses and response error payloads using logging.error with relevant contextual parameters.

### Verify

```bash
# Discover and run the repository static analysis suite to verify logging calls follow required conventions.
# Discover and execute test suites covering failure paths in external client interactions.
```

**Accept when:**
- Static analysis validates that error and lifecycle events invoke the standard logging interface without linting violations.
- Automated tests confirm error responses and configuration events are properly recorded in log outputs.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>