# Internal Module Adoption: crate::serializers::SerializationState: Serializer Construction Routines Utilize Crate Definitions

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SERIALIZERS-001** MUST: Serializer construction routines MUST utilize crate::definitions::DefinitionsBuilder to register and resolve recursive definition references across serialization boundaries.

### Verify

```bash
# Discover and run the project's primary test runner covering the serialization subsystem
# Discover and run the project's static analysis and linting entry point for type verification on all serializer implementations
```

**Accept when:**
- All serializer test suites pass without regressions across nested and recursive serialization test cases.
- Type checking and static verification pass with zero warnings across all serializer module definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All serializer modifications must use crate::serializers::SerializationState and crate::definitions::DefinitionsBuilder according to these rules.
</enforcement>