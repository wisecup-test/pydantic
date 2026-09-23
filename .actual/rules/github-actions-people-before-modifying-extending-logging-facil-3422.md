# Standard Library logging Adoption for Operational Script Diagnostics: Before Modifying Extending Logging Facilities Operational

These rules are ALWAYS ACTIVE for operational scripts and automation tasks performing external HTTP requests and configuration loading.

### Rules

- **R-LOG-001** MUST: Before modifying or extending logging facilities in operational scripts, the consumer MUST inspect the repository dependency configuration to resolve the environmental logging dependencies.
- **R-LOG-002** MUST: Initialize log formatting and severity levels at the entry point of operational scripts prior to executing external client calls.
- **R-LOG-003** MUST: Extract and sanitize error messages from response structures before passing them to logging invocations.

### Verify

```bash
# Discover and run the repository static analysis suite to verify logging calls follow required conventions
# Discover and execute test suites covering failure paths in external client interactions
```

**Accept when:**
- Static analysis validates that error and lifecycle events invoke the standard logging interface without linting violations.
- Automated tests confirm error responses and configuration events are properly recorded in log outputs.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>