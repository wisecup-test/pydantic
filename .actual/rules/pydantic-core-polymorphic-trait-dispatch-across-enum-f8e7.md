# Adoption of enum_dispatch for Static Polymorphic Dispatch: Polymorphic Trait Dispatch Across Enum Variants

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ED-001** MUST: Polymorphic trait dispatch across enum variants representing validators or garbage collection traversable components MUST use the enum_dispatch attribute macro on both trait definitions and wrapping enums.

### Verify

```bash
# Discover and run the project's compilation and verification tasks to confirm macro expansions and trait bindings resolve.
cargo check
cargo test
```

**Accept when:**
- All trait methods invoke the correct variant implementations through static dispatch without runtime errors.
- Compilation passes without macro expansion warnings or missing trait implementation errors across all configured build targets.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration compile-time type checking, test suite execution, and peer code reviews ensuring polymorphic paths do not introduce unneeded dynamic trait objects.
</enforcement>