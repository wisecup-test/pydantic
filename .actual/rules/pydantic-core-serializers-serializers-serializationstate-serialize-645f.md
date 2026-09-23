# Standardization on crate::serializers::SerializationState for Serializer Context Management: Schema Level Serializer Initializers Coordinate Recursive

These rules are ALWAYS ACTIVE for all schema serializers, prebuilt serializers, and type serializers within the serialization subsystem.

### Rules

- **R-SERIALIZE-001** SHOULD: Schema-level serializer initializers SHOULD coordinate recursive type resolution and reference tracking using DefinitionsBuilder in conjunction with crate::serializers::SerializationState.

### Verify

```bash
# Discover the test runner from the repository configuration and execute unit and integration suites targeting serialization modules.
# Discover the repository static analysis and linting verification tasks to ensure all serializer signatures adhere to state passing rules.
```

**Accept when:**
- All serializer test suites execute successfully with no regression in serialization performance or correctness.
- Static analysis verification completes with zero warnings regarding inconsistent serialization state arguments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites, static analysis checks, and peer review on pull requests touching serializer definitions.
</enforcement>