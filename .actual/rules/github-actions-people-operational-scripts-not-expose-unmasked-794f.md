# Standard Library logging Adoption for Operational Script Diagnostics: Operational Scripts Not Expose Unmasked Secret

These rules are ALWAYS ACTIVE for operational scripts and automation tasks performing external HTTP requests and configuration loading.

### Rules

- **R-OPS-001** MUST_NOT: Operational scripts MUST_NOT expose unmasked secret tokens or private credentials when emitting diagnostic log records.

### Verify

```bash
# Discover and run the repository static analysis suite to verify logging calls follow required conventions.
# Discover and execute test suites covering failure paths in external client interactions.
```

**Accept when:**
- Static analysis validates that error and lifecycle events invoke the standard logging interface without linting violations.
- Automated tests confirm error responses and configuration events are properly recorded in log outputs.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis, linting checks, and automated integration workflows.
</enforcement>