# Adoption of enum Module for String Enumerations and Configuration Protocols: Modules Handling Serialization Support Extracting String

These rules are ALWAYS ACTIVE for model configuration, input parsing, and serialization modules handling categorical configuration choices and protocol selectors.

### Rules

- **R-ENUM-001** SHOULD: Modules handling serialization SHOULD support extracting the string value directly from Enum instances or through configuration settings enabling enum value extraction.
- **R-ENUM-002** MANDATORY: Inherit from both str and Enum when defining categorical options that interface with serialization systems to preserve string compatibility.
- **R-ENUM-003** MANDATORY: Use Enum member references in configuration dictionaries and contract signatures to ensure autocompletion and static type checking.
- **R-ENUM-004** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository following the LOCK-VERSION GROUNDING procedure before writing code using versioned libraries.

### Verify

```bash
# Discover the repository test runner from project configuration and execute the test suite covering configuration and parsing modules.
# Discover the repository type checking suite from project configuration and execute type analysis to verify enum member typing across model definitions.
# Discover the repository linting and static analysis configuration to verify adherence to enumeration usage standards.
```

**Accept when:**
- All configuration and parsing test suites execute successfully without enumeration value mismatches.
- Static type analysis passes with zero type errors regarding enumeration member assignments and parameter types.
- All defined string enumeration classes successfully serialize and deserialize through serialization encoders.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipelines executing test suites and static type checkers, and peer code reviews verifying that new categorical options define string Enum classes rather than raw string literals.
</enforcement>