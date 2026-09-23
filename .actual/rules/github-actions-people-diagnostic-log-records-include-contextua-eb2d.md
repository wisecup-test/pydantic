# Standard Library logging Adoption for Operational Script Diagnostics: Diagnostic Log Records Include Contextual Iteration

These rules are ALWAYS ACTIVE for operational scripts and automation tasks performing external HTTP requests and configuration loading.

### Rules

- **R-LOG-001** SHOULD: Diagnostic log records SHOULD include contextual iteration markers and response status codes to facilitate failure triage.
- **R-LOG-002** MANDATORY: Initialize log formatting and severity levels at the entry point of operational scripts prior to executing external client calls.
- **R-LOG-003** MANDATORY: Extract and sanitize error messages from response structures before passing them to logging invocations to prevent leaking sensitive secrets or tokens.
- **R-LOG-004** MANDATORY: Execute lock-version grounding before writing code that uses a versioned library: find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for the exact version, and confirm APIs exist.

### Verify

```bash
# Discover and run the repository static analysis suite to verify logging calls follow required conventions.
# Discover and execute test suites covering failure paths in external client interactions.
```

**Accept when:**
- Static analysis validates that error and lifecycle events invoke the standard logging interface without linting violations.
- Automated tests confirm error responses and configuration events are properly recorded in log outputs.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, linting checks, and peer code review.
</enforcement>