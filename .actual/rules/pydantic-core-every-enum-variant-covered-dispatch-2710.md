# Adoption of enum_dispatch for Static Polymorphic Dispatch: Every Enum Variant Covered Dispatch Attribute

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENM-001** MUST: Every enum variant covered by an enum_dispatch attribute MUST implement the annotated dispatch trait directly.

### Verify

```bash
# Discover and run the project's static analysis and compilation verification tasks to confirm macro expansions and trait bindings resolve without errors.
# Execute the project's automated test suite to validate that static dispatch calls execute accurately across all enum variants.
```

**Accept when:**
- All trait methods invoke the correct variant implementations through static dispatch without runtime errors.
- Compilation passes without macro expansion warnings or missing trait implementation errors across all configured build targets.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration compile-time type checking, test suite execution, and peer code reviews ensuring polymorphic paths do not introduce unneeded dynamic trait objects.
</enforcement>