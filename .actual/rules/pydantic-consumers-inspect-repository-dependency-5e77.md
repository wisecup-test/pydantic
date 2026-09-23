# Adoption of pydantic_core.core_schema for Core Schema Representation and Traversal: Consumers Inspect Repository Dependency Manifest Authoritative

These rules are ALWAYS ACTIVE for all code implementing core schema representation, traversal, reference collection, or package entry-point export of core schema types.

### Rules

- **R-CORE-001** MUST: Consumers MUST inspect the repository dependency manifest and authoritative lock artifact to resolve the exact locked version of the core validation library before compiling or referencing core schema definitions.
- **R-CORE-002** MUST: Traverse core schemas recursively using dedicated traversal callbacks for schema definitions, references, and metadata dictionaries.
- **R-CORE-003** MUST: Maintain schema reference definitions within a dedicated context mapping to distinguish between inlinable single-reference schemas and shared definitions.

### Verify

```bash
sh -c 'test -n "$(find . -maxdepth 3 -type f \( -name "*test*" -o -name "*check*" \) | head -n 1)"' && \
sh -c 'echo "Discover and run repository verification scripts validating core schema traversal and export contracts"'
```

**Accept when:**
- The repository test suite executes successfully with all schema traversal and definition gathering tests passing.
- Schema reference collection accurately identifies inlinable definitions and reports missing definitions without unhandled exceptions.
- All package-level dynamic attribute exports for core schema types resolve without import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites, static type analysis, and peer code review enforce conformance; violations are blocked from merging.
</enforcement>