# Removal of eval_type_backport runtime dependency: Schema Generation Model Construction Modules Handle

These rules are ALWAYS ACTIVE for all schema generation and model construction modules involving forward reference evaluation.

### Rules

- **R-SCHEMA-001** MUST: Schema generation and model construction modules MUST handle forward reference evaluation exclusively through internal typing resolution routines.

### Verify

```bash
# Verify eval_type_backport is not present in runtime manifests
python -c "import tomli; data = tomli.load(open('pyproject.toml', 'rb')); assert 'eval_type_backport' not in data.get('project', {}).get('dependencies', [])"
# Execute the test suite covering forward references, type adapters, and model construction
pytest tests/test_forward_ref.py tests/test_main.py tests/test_type_adapter.py tests/test_types_namedtuple.py tests/test_typing.py tests/test_validate_call.py
```

**Accept when:**
- Project dependency definitions and lockfiles contain no reference to eval_type_backport in runtime sections
- All forward reference, typing, and schema construction tests pass successfully without external backport dependencies

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated CI dependency audits and test suites must ensure eval_type_backport is absent and internal type resolution functions correctly.
</enforcement>