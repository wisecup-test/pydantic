# LookupPath Module Adoption for Input Traversal and Key Resolution: Developers Inspect Project Lock Artifact Resolve

These rules are ALWAYS ACTIVE for all input processing, abstraction definitions, and concrete input representation modules.

### Rules

- **R-LUT-001** MUST: Developers MUST inspect the project lock artifact to resolve and verify the exact dependency configuration before introducing or updating input processing and lookup path dependencies.
- **R-LUT-002** MUST: All input processing modules route key and nested attribute resolution through LookupPath contracts.
- **R-LUT-003** MUST: Concrete input types handle both scalar keys and composite paths through the unified LookupPath resolution interface.
- **R-LUT-004** MUST: Preserve lookup failure semantics consistently so downstream validation layers receive standardized path missing signals.

### Verify

```bash
# Discover and execute the project test runner to validate input traversal test suites.
# Run the project static analysis and linting verification suite to ensure compliance with LookupPath interface contracts.
```

**Accept when:**
- All input processing modules route key and nested attribute resolution through LookupPath contracts.
- Project verification suites pass without regression in input lookup or validation behavior.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>