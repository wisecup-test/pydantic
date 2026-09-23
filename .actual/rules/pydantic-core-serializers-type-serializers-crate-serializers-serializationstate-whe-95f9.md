# Internal Module Adoption: crate::serializers::SerializationState: When Interacting External Host Runtime Data

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SER-001** MUST: When interacting with external or host runtime data types, serializer routines MUST resolve and verify the active dependency lock artifact in the project repository to bind exact dependency interfaces before invoking third-party integration points.

### Verify

```bash
# Discover the project's primary test runner from repository configuration and execute the complete test suite covering the serialization subsystem.
# Discover the project's static analysis and linting entry point from repository metadata and run type verification on all serializer implementations.
```

**Accept when:**
- All serializer test suites pass without regressions across nested and recursive serialization test cases.
- Type checking and static verification pass with zero warnings across all serializer module definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>