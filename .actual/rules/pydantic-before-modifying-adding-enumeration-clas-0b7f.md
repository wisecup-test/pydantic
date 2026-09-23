# Adoption of enum Module for String Enumerations and Configuration Protocols: Before Modifying Adding Enumeration Classes Relying

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENUM-001** MUST: Before modifying or adding enumeration classes relying on external or runtime libraries, the consumer MUST inspect the dependency lock artifact to confirm target environment compatibility and API support.
- **R-ENUM-002** MUST: Inherit from both str and Enum when defining categorical options that interface with serialization systems to preserve string compatibility.
- **R-ENUM-003** MUST: Use Enum member references in configuration dictionaries and contract signatures to ensure autocompletion and static type checking.
- **R-ENUM-004** MUST: Execute the lock-version grounding sequence (discover manifest, build tool, inspect lock/resolution artifact, look up official version-specific docs, confirm every API/class/function exists) before writing code that uses a versioned library.

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
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipelines executing test suites, static type checkers, and peer code reviews.
</enforcement>