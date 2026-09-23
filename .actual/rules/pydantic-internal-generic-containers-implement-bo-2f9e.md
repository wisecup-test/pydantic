# Standard Library types Module Adoption for Runtime Type Reflection and Descriptor Dispatch: Internal Generic Containers Implement Bounded Dictionary

These rules are ALWAYS ACTIVE for core framework modules performing dynamic class construction, method decoration, generic parameterization, or runtime type inspection.

### Rules

- **R-TYP-001** MAY: Internal generic containers MAY implement bounded dictionary caches to prevent unbounded memory growth during repeated generic submodel creation.
- **R-TYP-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and versions from the repository.
- **R-TYP-003** MANDATORY: Before writing code using a versioned library, execute the lock-version grounding process (find dependency manifest, identify build tool, inspect lock artifact, look up official documentation for exact version, confirm APIs exist, re-run per dependency at point of use).
- **R-TYP-004** MANDATORY: When constructing descriptor proxies, ensure descriptor protocol methods such as attribute binding and setter delegation operate transparently on wrapped functions.
- **R-TYP-005** MANDATORY: Ensure type reflection operations in generic parameterization handle composite arguments and unhashable structures cleanly before attempting cache key resolution.

### Verify

```bash
# Discover the repository build configuration and execute the primary automated test suite governing internal reflection, decorators, and generic models
# Discover and run the project type checking verification task to ensure all descriptor proxies and generic type wrappers satisfy static typing contracts
# Discover and run the project linting and structural analysis suites to detect prohibited reflection patterns or unregistered dependencies
```

**Accept when:**
- All automated tests covering generic model creation, class decorators, and type adapters pass cleanly without reflection errors
- Static type analysis across decorator definitions and generic submodel generators completes with zero diagnostic errors
- Dynamic descriptor proxies preserve transparent attribute access and binding semantics across model hierarchies

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites, static type checker validation runs, and peer code reviews verify conformance. Pull requests introducing external reflection dependencies or bypassing descriptor proxies are blocked.
</enforcement>