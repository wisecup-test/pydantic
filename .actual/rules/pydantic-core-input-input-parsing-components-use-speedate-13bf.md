# Adoption of speedate MicrosecondsPrecisionOverflowBehavior for Consistent Datetime Input Parsing: Input Parsing Components Use Speedate Microsecondsprecisionoverflowbehavior

These rules are ALWAYS ACTIVE for all input validation modules responsible for parsing temporal strings or structured JSON fields into datetime structures, and any data transformation routines encountering sub-microsecond timestamp representations.

### Rules

- **R-PARSE-001** MUST: Input parsing components MUST use speedate::MicrosecondsPrecisionOverflowBehavior to configure and enforce consistent datetime sub-second precision overflow semantics across all payload parsing boundaries.

### Verify

```bash
# Discover the workspace dependency configuration and build verification script from the repository root, then execute the standard test suite to confirm datetime parsing behavior conforms to specification.
# Inspect the project validation commands to run type checking and static analysis across all input parsing units.
```

**Accept when:**
- All test suites validating temporal input parsing across string and structured object formats pass without regressions.
- Sub-microsecond timestamp inputs trigger deterministic overflow handling in accordance with the configured precision overflow behavior across all input parsing boundaries.
- Static verification and code checks across input parsing modules confirm that external datetime parsing types match resolved dependency specifications.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations detected during automated verification fail build pipelines and pull requests introducing ad-hoc datetime parsing or bypassing precision overflow settings are blocked.
</enforcement>