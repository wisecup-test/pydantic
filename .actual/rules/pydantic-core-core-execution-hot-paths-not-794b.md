# Adoption of enum_dispatch for Static Polymorphic Dispatch: Core Execution Hot Paths Not Use

These rules are ALWAYS ACTIVE for all code files involving polymorphic method dispatch across closed sets of types implementing shared traits, specifically core validation logic and garbage collection traversal contracts requiring zero runtime dynamic dispatch overhead.

### Rules

- **R-ENUM-001** MUST_NOT: Core execution hot paths MUST NOT use heap-allocated dynamic trait objects for polymorphism when variant sets can be statically enumerated.

### Verify

```bash
# Discover and run the project's static analysis and compilation verification tasks
# to confirm macro expansions and trait bindings resolve without errors.
# Execute the project's automated test suite to validate that static dispatch calls execute accurately across all enum variants.
cargo check
cargo test
```

**Accept when:**
- All trait methods invoke the correct variant implementations through static dispatch without runtime errors.
- Compilation passes without macro expansion warnings or missing trait implementation errors across all configured build targets.

<enforcement>
Claude Code MUST NOT skip or defer verification. All polymorphic paths in core execution hot paths must use static dispatch rather than heap-allocated dynamic trait objects.
</enforcement>