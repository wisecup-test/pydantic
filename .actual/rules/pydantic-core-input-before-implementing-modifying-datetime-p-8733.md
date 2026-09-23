# Adoption of speedate MicrosecondsPrecisionOverflowBehavior for Consistent Datetime Input Parsing: Before Implementing Modifying Datetime Parsing Logic

These rules are ALWAYS ACTIVE for all input validation modules responsible for parsing temporal strings or structured JSON fields into datetime structures, and any data transformation routines encountering sub-microsecond timestamp representations.

### Rules

- **R-DATETIME-001** MUST: Before implementing or modifying datetime parsing logic relying on external parsing crates, consumers MUST locate the repository dependency lock artifact to determine the exact resolved version and verify the public contract of the dependency against official release documentation.
- **R-DATETIME-002** MUST: Configure datetime parsing invocations to pass uniform precision overflow settings across all input source adapters.
- **R-DATETIME-003** MUST: Map library-specific precision overflow parse errors directly into standard validation error responses.

### Verify

```bash
# Discover workspace dependency configuration and build verification script from repository root, then execute tests and type checking:
cargo test
cargo check
```

**Accept when:**
- All test suites validating temporal input parsing across string and structured object formats pass without regressions.
- Sub-microsecond timestamp inputs trigger deterministic overflow handling in accordance with the configured precision overflow behavior across all input parsing boundaries.
- Static verification and code checks across input parsing modules confirm that external datetime parsing types match resolved dependency specifications.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations detected during automated verification fail build pipelines.
</enforcement>