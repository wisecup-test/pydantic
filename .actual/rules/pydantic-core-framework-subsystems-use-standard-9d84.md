# Standard Library types Module Adoption for Runtime Type Reflection and Descriptor Dispatch: Core Framework Subsystems Use Standard Library

These rules are ALWAYS ACTIVE for all core validation and data modeling frameworks requiring runtime introspection of types, callable signatures, and class attributes.

### Rules

- **R-TYPES-001** MUST: Core framework subsystems MUST use the standard library types module for runtime type reflection, dynamic function introspection, and descriptor proxying rather than relying on external reflection libraries or arbitrary string evaluations.

### Verify

```bash
# Discover the repository build configuration and execute the primary automated test suite
# Discover and run the project type checking verification task
# Discover and run the project linting and structural analysis suites
```

**Accept when:**
- All automated tests covering generic model creation, class decorators, and type adapters pass cleanly without reflection errors
- Static type analysis across decorator definitions and generic submodel generators completes with zero diagnostic errors
- Dynamic descriptor proxies preserve transparent attribute access and binding semantics across model hierarchies

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing external reflection dependencies or bypassing established descriptor proxy structures must be blocked.
</enforcement>