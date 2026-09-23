# Adoption of speedate MicrosecondsPrecisionOverflowBehavior for Consistent Datetime Input Parsing: Parsing Layers Propagate Precision Overflow Failures

These rules are ALWAYS ACTIVE for all input validation modules responsible for parsing temporal strings or structured JSON fields into datetime structures, and any data transformation routines encountering sub-microsecond timestamp representations.

### Rules

- **R-DATETIME-001** SHOULD: Parsing layers SHOULD propagate precision overflow failures as structured input validation errors rather than panicking or silently truncating fractional second values.
- **R-DATETIME-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-DATETIME-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding sequence in order: find the dependency manifest, identify the build tool, inspect the lock/resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version's docs, and re-run steps per dependency at point of use for version-sensitive behavior.
- **R-DATETIME-004** MANDATORY: Configure datetime parsing invocations to pass uniform precision overflow settings across all input source adapters.
- **R-DATETIME-005** MANDATORY: Map library-specific precision overflow parse errors directly into standard validation error responses.

### Verify

```bash
# Discover workspace dependency configuration and build verification script from repository root
# Execute standard test suite to confirm datetime parsing behavior conforms to specification
# Run type checking and static analysis across all input parsing units
```

**Accept when:**
- All test suites validating temporal input parsing across string and structured object formats pass without regressions.
- Sub-microsecond timestamp inputs trigger deterministic overflow handling in accordance with the configured precision overflow behavior across all input parsing boundaries.
- Static verification and code checks across input parsing modules confirm that external datetime parsing types match resolved dependency specifications.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations detected during automated verification fail build pipelines, and pull requests introducing ad-hoc datetime parsing or bypassing precision overflow settings are blocked until aligned with standardized library usage.
</enforcement>