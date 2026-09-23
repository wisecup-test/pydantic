# Standard Library types Module Adoption for Runtime Type Reflection and Descriptor Dispatch: Generic Type Specialization Routines Not Bypass

These rules are ALWAYS ACTIVE for all core framework modules performing dynamic class construction, method decoration, generic parameterization, or runtime type inspection.

### Rules

- **R-REF-001** MUST_NOT: Generic type specialization routines MUST NOT bypass type reflection caches when instantiating parameterized models with identical type variable arguments.

### Verify

```bash
# Discover and execute the primary automated test suite governing internal reflection, decorators, and generic models
# Discover and run the project type checking verification task to ensure all descriptor proxies and generic type wrappers satisfy static typing contracts
# Discover and run the project linting and structural analysis suites to detect prohibited reflection patterns or unregistered dependencies
```

**Accept when:**
- All automated tests covering generic model creation, class decorators, and type adapters pass cleanly without reflection errors
- Static type analysis across decorator definitions and generic submodel generators completes with zero diagnostic errors
- Dynamic descriptor proxies preserve transparent attribute access and binding semantics across model hierarchies

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>