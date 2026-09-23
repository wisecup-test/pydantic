# dataclasses Module Integration for Field Extraction and Signature Generation: Custom Field Annotations Dataclass Structures Inherit

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DC-001** SHOULD: Custom field annotations on dataclass structures SHOULD inherit from PydanticMetadata or wrap metadata using pydantic_general_metadata to preserve introspection attributes across model rebuilds.
- **R-DC-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-DC-003** MANDATORY: Before writing code that uses a versioned library, the consumer MUST follow the LOCK-VERSION GROUNDING procedure: find dependency manifest, identify build tool, inspect lock/resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use.
- **R-DC-004** MANDATORY: Implementers must ensure that collect_dataclass_fields returns field mappings compatible with the metadata expectations of generate_pydantic_signature.
- **R-DC-005** MANDATORY: Verification of parameter identifiers must precede signature parameter construction to avoid syntax errors with non-standard field names.

### Verify

```bash
# Discover the test runner configuration from the repository manifest and execute the test suite covering field reflection and dataclass extraction.
# Locate the static analysis configuration and run the type checker and linter across modules implementing signature synthesis contracts.
```

**Accept when:**
- Dataclass field extraction correctly populates field metadata structures with aliases and validation markers.
- Generated constructor signatures match dataclass parameter definitions without raising evaluation errors.
- All discovered test suites and static analysis checks pass with zero violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test suites and peer review enforce architectural boundaries between field collection and signature generation. Pull requests bypassing collect_dataclass_fields or generate_pydantic_signature must be blocked until compliant, and non-compliant reflection implementations must be refactored.
</enforcement>