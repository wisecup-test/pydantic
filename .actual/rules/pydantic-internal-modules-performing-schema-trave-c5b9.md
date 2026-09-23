# Adoption of pydantic_core.core_schema for Core Schema Representation and Traversal: Internal Modules Performing Schema Traversal Reference

These rules are ALWAYS ACTIVE for all internal modules performing schema traversal, reference collection, or schema cleaning, as well as package entry points exporting core schema types and dynamic attribute resolution hooks.

### Rules

- **R-PS-001** MUST: Internal modules performing schema traversal, reference collection, or schema cleaning MUST adopt pydantic_core.core_schema data structures as the authoritative intermediate representation for schema manipulation.

### Verify

```bash
sh -c 'test -n "$(find . -maxdepth 3 -type f \( -name "*test*" -o -name "*check*" \) | head -n 1)"'
sh -c 'echo "Discover and run repository verification scripts validating core schema traversal and export contracts"'
```

**Accept when:**
- The repository test suite executes successfully with all schema traversal and definition gathering tests passing.
- Schema reference collection accurately identifies inlinable definitions and reports missing definitions without unhandled exceptions.
- All package-level dynamic attribute exports for core schema types resolve without import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via the specified test suite and static type analysis routines.
</enforcement>