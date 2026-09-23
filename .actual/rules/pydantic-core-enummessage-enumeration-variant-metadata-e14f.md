# Adoption of strum::EnumMessage for Enumeration Variant Metadata: Modules Defining Enumerations That Supply Descriptive

These rules are ALWAYS ACTIVE for input processing and validation enumeration types that expose variant-level descriptions or error messages, and public contract boundaries where enum variants correspond to structured error
 messages or argument types.

### Rules

- **R-STRUM-001** MUST: Modules defining enumerations that supply descriptive messages or error labels for input handling and validation MUST implement strum::EnumMessage via derive declarations.

### Verify

```bash
# Discover and execute the project's test suite to verify that enumeration message extraction behaves as expected across validation routines.
# Discover and run the project's static analysis and linting scripts to verify compliance with macro derive conventions.
```

**Accept when:**
- All enumerations requiring descriptive messages derive strum::EnumMessage without manual match dispatch routines.
- Project test suites and compilation checks pass cleanly with zero warnings or errors regarding enum message resolution.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration build and test pipelines and peer code review.
</enforcement>