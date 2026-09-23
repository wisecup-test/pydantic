# Standard Library types Module Adoption for Runtime Type Reflection and Descriptor Dispatch: Decorator Wrapping Layers Implement Transparent Descriptor

These rules are ALWAYS ACTIVE for core framework modules performing dynamic class construction, method decoration, generic parameterization, or runtime type inspection.

### Rules

- **R-ADR-001** MUST: Decorator wrapping layers MUST implement transparent descriptor proxies that delegate descriptor protocol methods to the underlying wrapped callable while preserving attribute access and descriptor binding semantics.

### Verify

```bash
# Discover and execute the primary automated test suite governing internal reflection, decorators, and generic models
# Discover and run the project type checking verification task
# Discover and run the project linting and structural analysis suites
```

**Accept when:**
- All automated tests covering generic model creation, class decorators, and type adapters pass cleanly without reflection errors
- Static type analysis across decorator definitions and generic submodel generators completes with zero diagnostic errors
- Dynamic descriptor proxies preserve transparent attribute access and binding semantics across model hierarchies

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites, static type checker validation runs, and peer code reviews verify adherence.
</enforcement>