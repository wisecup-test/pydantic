<rule_activation id="6fb3b0fc-cb17-4c5f-9a16-6fbe9b94bb6b" title="Adopt Pydantic V1 Compatibility Layer for Type Validation: Projects Use Typeadapter" applies_to="**/*">
These rules are ALWAYS ACTIVE for all Python modules requiring runtime type validation, data serialization, schema generation, API request/response validation, configuration parsing, and JSON schema generation.
</rule_activation>

### Rules

- **R-PYDANTIC-001** MAY: Projects MAY use TypeAdapter for dynamic type validation when model classes are not appropriate.
- **R-PYDANTIC-002** MUST: All Pydantic V1 imports use the `pydantic.v1` namespace, not direct `pydantic` imports (e.g., `from pydantic.v1 import BaseModel`).
- **R-PYDANTIC-003** MUST: Type checking tests exist in `tests/typechecking/` directory covering TypeAdapter, JSON schema, and pipeline APIs.
- **R-PYDANTIC-004** SHOULD: Use Pydantic V2 TypeAdapter for dynamic validation: `TypeAdapter(MyType).validate_python(data)`.
- **R-PYDANTIC-005** SHOULD: Use `model_config` for Pydantic V2 configuration instead of Config class from V1.
- **R-PYDANTIC-006** SHOULD: Generate JSON schemas using `model.model_json_schema()` in V2 or `model.schema()` in V1 compatibility mode.
- **R-PYDANTIC-007** MUST NOT: Bypass validation by using unvalidated dictionaries or incorrect import paths without documented exception.

### Verify

```bash
# Verify all Pydantic V1 imports are properly namespaced
grep -r 'from pydantic import' --include='*.py' . | grep -v 'from pydantic.v1 import' | grep -v 'from pydantic import TypeAdapter' || echo 'V1 imports properly namespaced'

# Verify type checking tests exist
find . -path '*/tests/typechecking/*.py' -type f | wc -l | awk '{if($1>0) print "Type checking tests found"; else exit 1}'

# Verify Pydantic imports work without conflicts
python -c 'import pydantic; from pydantic.v1 import BaseModel; from pydantic import TypeAdapter; print("Pydantic imports successful")'
```

**Accept when:**
- All Pydantic V1 imports use the `pydantic.v1` namespace, not direct pydantic imports
- Type checking test files exist in `tests/typechecking/` directory covering TypeAdapter, JSON schema, and pipeline APIs
- Pydantic and `pydantic.v1` modules can be imported successfully without conflicts
- Validator tests demonstrate compatibility with pydantic-core validation engine
- All data models include validation logic for API request/response handling
- Configuration file parsing includes runtime validation

<enforcement>
Claude Code MUST NOT skip or defer verification. Pre-commit hooks, CI pipeline, and code review processes enforce these rules. Violations result in CI build failures, merge blocks, and linter errors. Exceptions require architecture review board approval with documented justification.
</enforcement>