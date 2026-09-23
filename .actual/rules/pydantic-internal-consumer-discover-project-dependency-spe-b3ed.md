# dataclasses Module Integration for Field Extraction and Signature Generation: Consumer Discover Project Dependency Specification Resolve

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DAT-001** MUST: The consumer MUST discover the project dependency specification and resolve the authoritative locked environment before applying changes to field reflection or signature synthesis routines.
- **R-DAT-002** MUST: Implementers must ensure that collect_dataclass_fields returns field mappings compatible with the metadata expectations of generate_pydantic_signature.
- **R-DAT-003** MUST: Verification of parameter identifiers must precede signature parameter construction to avoid syntax errors with non-standard field names.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>