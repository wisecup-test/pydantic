# Removal of eval_type_backport runtime dependency: Project Dependency Specifications Lockfiles Not Declare

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DEP-001** MUST_NOT: Project dependency specifications and lockfiles MUST NOT declare eval_type_backport as a runtime dependency.

### Verify

```bash
# Discover and execute the project dependency audit to verify eval_type_backport is not present in runtime manifests
# Discover and execute the test suite covering forward references, type adapters, and model construction
pytest tests/test_forward_ref.py tests/test_main.py tests/test_type_adapter.py tests/test_types_namedtuple.py tests/test_typing.py tests/test_validate_call.py
```

**Accept when:**
- Project dependency definitions and lockfiles contain no reference to eval_type_backport in runtime sections
- All forward reference, typing, and schema construction tests pass successfully without external backport dependencies

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>