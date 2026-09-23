# dataclasses Module Integration for Field Extraction and Signature Generation: Field Introspection Modules Not Evaluate Parameter

These rules are ALWAYS ACTIVE for all files matching the scope of field extraction, metadata collection routines, and dynamic callable signature synthesis for dataclasses and models.

### Rules

- **R-DCI-001** MUST_NOT: Field introspection modules MUST NOT evaluate parameter identifiers or field aliases without verifying identifier validity via is_valid_identifier.

### Verify

```bash
# Discover the test runner configuration from the repository manifest and execute the test suite covering field reflection and dataclass extraction.
# Locate the static analysis configuration and run the type checker and linter across modules implementing signature synthesis contracts.
pytest -k "dataclass or reflection or signature"
mypy . 
ruff check .
```

**Accept when:**
- Dataclass field extraction correctly populates field metadata structures with aliases and validation markers.
- Generated constructor signatures match dataclass parameter definitions without raising evaluation errors.
- All discovered test suites and static analysis checks pass with zero violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test suites and peer review enforce architectural boundaries between field collection and signature generation.
</enforcement>