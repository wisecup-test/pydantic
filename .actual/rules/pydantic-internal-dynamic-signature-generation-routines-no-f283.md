# dataclasses Module Integration for Field Extraction and Signature Generation: Dynamic Signature Generation Routines Not Bypass

These rules are ALWAYS ACTIVE for all code handling field extraction, metadata collection, and dynamic callable signature synthesis for models and dataclass containers.

### Rules

- **R-DC-001** MUST_NOT: Dynamic signature generation routines MUST NOT bypass defined field aliases when parameter names conflict with external client contracts.

### Verify

```bash
# Discover and execute the test runner covering field reflection and dataclass extraction
# Locate the static analysis configuration and run the type checker and linter across modules implementing signature synthesis contracts
```

**Accept when:**
- Dataclass field extraction correctly populates field metadata structures with aliases and validation markers.
- Generated constructor signatures match dataclass parameter definitions without raising evaluation errors.
- All discovered test suites and static analysis checks pass with zero violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration test suites and peer review enforce architectural boundaries between field collection and signature generation.
</enforcement>