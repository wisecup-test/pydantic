# Adoption of dataclasses Module for Structured Data Modeling and Field Introspection: Field Introspection Across Dataclass Types Use

These rules are ALWAYS ACTIVE for internal modules responsible for schema gathering, field reflection, and constructor signature generation, as well as data conversion and reporting utilities constructing structured table rows.

### Rules

- **R-DATACLASS-001** MUST: Field introspection across dataclass types MUST use dedicated collector functions such as collect_dataclass_fields to normalize field attributes into unified field metadata structures.

### Verify

```bash
# Discover and run the project test runner configuration from repository manifests covering schema gathering, field extraction, and signature generation
# Locate and run repository static type analysis and linter scripts to verify dataclass usage and metadata typing compliance
```

**Accept when:**
- All automated test suites verifying dataclass field extraction, schema traversal, and signature generation execute successfully without errors.
- Static type analysis verifies that all field metadata structures and gathered schema references conform to defined type contracts without violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration test suites, static type checking, and peer code review.
</enforcement>