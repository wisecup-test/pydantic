# Adoption of dataclasses Module for Structured Data Modeling and Field Introspection: Modules Defining Structured Internal State Introspecting

These rules are ALWAYS ACTIVE for modules defining structured internal state or introspecting data structures.

### Rules

- **R-DATACLASS-001** MUST: Modules defining structured internal state or introspecting data structures MUST adopt dataclasses module conventions to structure domain records and extract field definitions.

### Verify

```bash
# Discover and execute project test suite covering schema gathering, field extraction, and signature generation
# Locate and run repository static type analysis and linter scripts to verify dataclass usage and metadata typing compliance
```

**Accept when:**
- All automated test suites verifying dataclass field extraction, schema traversal, and signature generation execute successfully without errors.
- Static type analysis verifies that all field metadata structures and gathered schema references conform to defined type contracts without violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration test suites, static type checking, linting pipelines, and peer code review. Violations fail CI verification and require refactoring to utilize standard field collection utilities.
</enforcement>