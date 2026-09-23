# LookupPath Module Adoption for Input Traversal and Key Resolution: Concrete Input Implementations Not Introduce Bespoke

These rules are ALWAYS ACTIVE for input abstraction definitions, trait contracts governing data ingestion, and concrete input representation modules responsible for key resolution and value extraction.

### Rules

- **R-LKP-001** MUST NOT: Concrete input implementations introduce bespoke path traversal parsing logic that circumvents the LookupPath interface.

### Verify

```bash
# Discover and execute the project test runner to validate input traversal test suites.
# Run the project static analysis and linting verification suite to ensure compliance with LookupPath interface contracts.
```

**Accept when:**
- All input processing modules route key and nested attribute resolution through LookupPath contracts.
- Project verification suites pass without regression in input lookup or validation behavior.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suite execution, continuous integration verification checks, and peer code reviews validating that new input types implement LookupPath traversal contracts.
</enforcement>