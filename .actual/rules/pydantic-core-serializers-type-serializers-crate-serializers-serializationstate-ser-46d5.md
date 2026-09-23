# Internal Module Adoption: crate::serializers::SerializationState: Serializers Utilize Std Sync Arc Borrow

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SER-001** MAY: Serializers MAY utilize std::sync::Arc or std::borrow::Cow pointer primitives when storing reusable state within serializer structures.

### Verify

```bash
# Discover the project's primary test runner from repository configuration and execute the complete test suite covering the serialization subsystem.
# Discover the project's static analysis and linting entry point from repository metadata and run type verification on all serializer implementations.
```

**Accept when:**
- All serializer test suites pass without regressions across nested and recursive serialization test cases.
- Type checking and static verification pass with zero warnings across all serializer module definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipelines and architectural code review.
</enforcement>