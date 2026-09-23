# dataclasses Module Integration for Field Extraction and Signature Generation: Signature Generation Routines Utilize Generate Pydantic

These rules are ALWAYS ACTIVE for all code handling field extraction, metadata collection, and dynamic callable signature synthesis for models and dataclasses.

### Rules

- **R-DC-001** MUST: Signature generation routines MUST utilize generate_pydantic_signature and signature_no_eval to reflect dataclass parameter defaults, field aliases, and validation aliases without triggering premature annotation evaluation.

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
Claude Code MUST NOT skip or defer verification. Pull requests bypassing collect_dataclass_fields or generate_pydantic_signature must be blocked until compliant.
</enforcement>