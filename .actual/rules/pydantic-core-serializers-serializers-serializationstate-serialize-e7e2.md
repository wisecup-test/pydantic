# Standardization on crate::serializers::SerializationState for Serializer Context Management: Type Serializers Query Crate Serializationstate Conditionally

These rules are ALWAYS ACTIVE for schema serializers, prebuilt serializers, and type serializers within the serialization subsystem requiring traversal state, recursion control, or configuration flags.

### Rules

- **R-SER-001** MAY: Type serializers MAY query crate::serializers::SerializationState to conditionally adjust serialization output formats based on configured runtime options.
- **R-SER-002** MANDATORY: When defining new type serializers, implement serialization methods to receive crate::serializers::SerializationState as an active parameter and pass it into child serializer calls.
- **R-SER-003** MANDATORY: Coordinate definitions resolution by integrating DefinitionsBuilder during schema preparation to ensure type serializers resolve recursive identifiers through crate::serializers::SerializationState.
- **R-SER-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool from the manifest. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up official documentation, changelog, or public API reference for that exact version. (5) Confirm every API, class, or function called exists in that exact version's documentation. (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.

### Verify

```bash
# Discover the test runner from the repository configuration and execute unit and integration suites targeting serialization modules.
# Discover the repository static analysis and linting verification tasks to ensure all serializer signatures adhere to state passing rules.
```

**Accept when:**
- All serializer test suites execute successfully with no regression in serialization performance or correctness.
- Static analysis verification completes with zero warnings regarding inconsistent serialization state arguments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites, static analysis checks, and peer reviews verify serializer output consistency across nested types. Pull requests introducing serializers that bypass crate::serializers::SerializationState will fail automated checks and review approval.
</enforcement>