# Standard Library types Module Adoption for Runtime Type Reflection and Descriptor Dispatch: Subsystems Unwrapping Decorated Functions Verify Callable

These rules are ALWAYS ACTIVE for all core framework modules performing dynamic class construction, method decoration, generic parameterization, or runtime type inspection.

### Rules

- **R-ADR-001** SHOULD: Subsystems unwrapping decorated functions SHOULD verify callable interfaces and unwrap nested descriptors safely before passing functions to schema generators.
- **R-ADR-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ADR-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, re-run per dependency at point of use.
- **R-ADR-004** MANDATORY: When constructing descriptor proxies, ensure descriptor protocol methods such as attribute binding and setter delegation operate transparently on wrapped functions.
- **R-ADR-005** MANDATORY: Ensure type reflection operations in generic parameterization handle composite arguments and unhashable structures cleanly before attempting cache key resolution.

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
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites, static type checker validation runs, and peer code reviews verify compliance. Violations will block pull requests that introduce external reflection dependencies or bypass established descriptor proxy structures, and flag regressions in generic type caching or descriptor binding.
</enforcement>