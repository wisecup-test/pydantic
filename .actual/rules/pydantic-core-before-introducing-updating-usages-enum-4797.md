# Adoption of enum_dispatch for Static Polymorphic Dispatch: Before Introducing Updating Usages Enum Dispatch

These rules are ALWAYS ACTIVE for code implementing or interacting with validation logic, garbage collection traversal routines, and polymorphic method dispatch across closed variant sets.

### Rules

- **R-ED-001** MUST: Before introducing or updating usages of enum_dispatch, consumers MUST discover the project dependency manifest and lock artifact to resolve the exact locked dependency version and verify API compatibility against official documentation.
- **R-ED-002** MUST: Apply the enum_dispatch attribute to the trait definition before applying it to the corresponding enum definition that aggregates the variant structs.
- **R-ED-003** MUST: Ensure every variant struct implements all functions of the annotated trait prior to compiling the enclosing dispatch enum.

### Verify

```bash
# Discover and run the project's static analysis and compilation verification tasks
# (e.g., cargo check / cargo test, derived from repository manifest)
cargo check --all-targets
cargo test
```

**Accept when:**
- All trait methods invoke the correct variant implementations through static dispatch without runtime errors.
- Compilation passes without macro expansion warnings or missing trait implementation errors across all configured build targets.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>