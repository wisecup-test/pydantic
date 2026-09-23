# Adoption of enum Module for String Enumerations and Configuration Protocols: Functions Consuming Categorical Options Compare Input

These rules are ALWAYS ACTIVE for model configuration, input parsing, and serialization modules handling discrete categorical options.

### Rules

- **R-ENUM-001** SHOULD: Functions consuming categorical options SHOULD compare input values against Enum members or use Enum validation before executing branch logic.
- **R-ENUM-002** MANDATORY: DISCOVERY POLICY: This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.
- **R-ENUM-003** MANDATORY: LOCK-VERSION GROUNDING — before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use.
- **R-ENUM-004** MANDATORY: Inherit from both str and Enum when defining categorical options that interface with serialization systems to preserve string compatibility.
- **R-ENUM-005** MANDATORY: Use Enum member references in configuration dictionaries and contract signatures to ensure autocompletion and static type checking.

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