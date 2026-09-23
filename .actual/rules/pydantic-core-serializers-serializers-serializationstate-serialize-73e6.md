# Standardization on crate::serializers::SerializationState for Serializer Context Management: Serializer Implementations Within Serialization Subsystem Accept

These rules are ALWAYS ACTIVE for all schema serializers, prebuilt serializers, and type serializers within the serialization subsystem.

### Rules

- **R-SERIALIZERS-001** MUST: All serializer implementations within the serialization subsystem MUST accept and thread crate::serializers::SerializationState through serialization execution paths to maintain consistent execution context.

### Verify

```bash
# Discover the test runner from repository configuration and execute tests targeting serialization modules
# Discover repository static analysis and linting verification tasks to ensure all serializer signatures adhere to state passing rules
```

**Accept when:**
- All serializer test suites execute successfully with no regression in serialization performance or correctness.
- Static analysis verification completes with zero warnings regarding inconsistent serialization state arguments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing serializers that bypass crate::serializers::SerializationState will fail automated checks and review approval. Non-compliant signatures must be refactored to thread the standard state object prior to merge.
</enforcement>