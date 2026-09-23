# Adoption of pydantic_core.core_schema for Core Schema Representation and Traversal: Schema Traversal Routines Implement Visitor Functions

These rules are ALWAYS ACTIVE for internal modules performing schema traversal, reference collection, or schema cleaning, and package entry points exporting core schema types and dynamic attribute resolution hooks.

### Rules

- **R-PYDANTIC-001** MUST: Schema traversal routines MUST implement visitor functions for metadata and definition references using typed collection structures conforming to GatherResult and raise MissingDefinitionError when resolving undefined schema references.

### Verify

```bash
sh -c 'test -n "$(find . -maxdepth 3 -type f \( -name "*test*" -o -name "*check*" \) | head -n 1)"
sh -c 'echo "Discover and run repository verification scripts validating core schema traversal and export contracts"
```

**Accept when:**
- The repository test suite executes successfully with all schema traversal and definition gathering tests passing.
- Schema reference collection accurately identifies inlinable definitions and reports missing definitions without unhandled exceptions.
- All package-level dynamic attribute exports for core schema types resolve without import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>