# Standard Library types Module Adoption for Runtime Type Reflection and Descriptor Dispatch: Before Implementing Modifying Type Reflection Logic

These rules are ALWAYS ACTIVE for core framework modules performing dynamic class construction, method decoration, generic parameterization, or runtime type inspection.

### Rules

- **R-REF-001** MUST: Before implementing or modifying type reflection logic involving external dependencies, developers MUST discover the dependency manifest and authoritative lock artifact in the project repository to verify resolved library versions against official documentation.
- **R-REF-002** MUST: Execute lock-version grounding steps in order (find manifest, identify build tool, inspect lock/resolution artifact, look up official docs for that exact version, and confirm every API/class/function exists in that version before use).
- **R-REF-003** MUST: Construct descriptor proxies such that descriptor protocol methods (attribute binding, setter delegation) operate transparently on wrapped functions.
- **R-REF-004** MUST: Handle composite arguments and unhashable structures cleanly in type reflection operations before attempting cache key resolution.

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
Claude Code MUST NOT skip or defer verification. All changes involving runtime type reflection, class decorators, or descriptor dispatch must strictly adhere to lock-version grounding and transparent proxy design.
</enforcement>