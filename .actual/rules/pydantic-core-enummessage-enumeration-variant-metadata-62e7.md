# Adoption of strum::EnumMessage for Enumeration Variant Metadata: Enumerations Purely Internal Machine Oriented Discriminant

These rules are ALWAYS ACTIVE for all input processing and validation enumeration types that expose variant-level descriptions or error messages, and public contract boundaries where enum variants correspond to structured error messages or argument types.

### Rules

- **R-STRUM-001** MAY: Enumerations with purely internal machine-oriented discriminant semantics and no client-facing message requirements MAY omit strum::EnumMessage derivations.

### Verify

```bash
# Discover and execute the project's test suite to verify that enumeration message extraction behaves as expected across validation routines.
# Discover and run the project's static analysis and linting scripts to verify compliance with macro derive conventions.
```

**Accept when:**
- All enumerations requiring descriptive messages derive strum::EnumMessage without manual match dispatch routines.
- Project test suites and compilation checks pass cleanly with zero warnings or errors regarding enum message resolution.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>