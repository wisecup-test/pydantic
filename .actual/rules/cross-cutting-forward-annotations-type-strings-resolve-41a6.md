# Removal of eval_type_backport runtime dependency: Forward Annotations Type Strings Resolved Python

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TYP-001** MUST: Forward annotations and type strings MUST be resolved using Python standard library typing and internal typing utilities without third-party evaluation backports.

### Verify

```bash
# Verify eval_type_backport is not present in runtime manifests
python -c "import tomllib; data = tomllib.load(open('pyproject.toml', 'rb')); assert 'eval_type_backport' not in data.get('project', {}).get('dependencies', [])"

# Run the test suite covering forward references, type adapters, and model construction
pytest tests/test_forward_ref.py tests/test_main.py tests/test_type_adapter.py tests/test_types_namedtuple.py tests/test_typing.py tests/test_validate_call.py
```

**Accept when:**
- Project dependency definitions and lockfiles contain no reference to eval_type_backport in runtime sections
- All forward reference, typing, and schema construction tests pass successfully without external backport dependencies

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>