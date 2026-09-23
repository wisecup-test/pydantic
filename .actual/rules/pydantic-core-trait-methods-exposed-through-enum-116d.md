# Adoption of enum_dispatch for Static Polymorphic Dispatch: Trait Methods Exposed Through Enum Dispatch

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENUM-001** SHOULD: Trait methods exposed through enum_dispatch SHOULD avoid manual match boilerplate in consuming modules and rely on the generated static dispatch expansions.

### Verify

```bash
# Discover and run the project's static analysis and compilation verification tasks to confirm macro expansions and trait bindings resolve without errors.
# Execute the project's automated test suite to validate that static dispatch calls execute accurately across all enum variants.
```

**Accept when:**
- All trait methods invoke the correct variant implementations through static dispatch without runtime errors.
- Compilation passes without macro expansion warnings or missing trait implementation errors across all configured build targets.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>