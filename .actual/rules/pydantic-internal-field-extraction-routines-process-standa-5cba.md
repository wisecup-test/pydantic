# dataclasses Module Integration for Field Extraction and Signature Generation: Field Extraction Routines Process Standard Dataclass

These rules are ALWAYS ACTIVE for all code handling field extraction, metadata collection, and dynamic signature synthesis for standard dataclasses and custom models.

### Rules

- **R-DC-001** MUST: Field extraction routines MUST process standard dataclass structures through collect_dataclass_fields while routing model definitions through collect_model_fields to isolate dataclass semantics from model field configuration.

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
Claude Code MUST NOT skip or defer verification. All pull requests bypassing collect_dataclass_fields or generate_pydantic_signature must be blocked until compliant.
</enforcement>