# Adoption of strum::EnumMessage for Enumeration Variant Metadata: Modules Not Implement Manual String Matching

These rules are ALWAYS ACTIVE for input processing and validation enumeration types that expose variant-level descriptions or error messages, and public contract boundaries where enum variants correspond to structured error messages or argument types.

### Rules

- **R-STRUM-001** MUST_NOT: Modules MUST NOT implement manual string-matching dispatch blocks to retrieve variant descriptions when an enumeration is annotated with strum::EnumMessage.

### Verify

```bash
# Discover and execute the project's test suite to verify that enumeration message extraction behaves as expected across validation routines.
# Discover and run the project's static analysis and linting scripts to verify compliance with macro derive conventions.
cargo test
cargo clippy
```

**Accept when:**
- All enumerations requiring descriptive messages derive strum::EnumMessage without manual match dispatch routines.
- Project test suites and compilation checks pass cleanly with zero warnings or errors regarding enum message resolution.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration build and test pipelines and peer code review.
</enforcement>