<rule_activation id="673dfb3b-148a-4e2c-bcf1-baba2a4df268" title="Implement Cache Layer for External API Data Validation: Cached Validation Results" applies_to="**/*">
These rules are ALWAYS ACTIVE for all files matching public-facing API endpoints and validation libraries that process network-based data types (URLs, email addresses, IP addresses, domain names).
</rule_activation>

### Rules

- **R-CACHE-001** MUST: Cached validation results MUST include both successful and failed validation outcomes to prevent repeated validation of known-invalid inputs.

### Verify

```bash
# Check for cache decorators or cache layer implementations in network validation modules
grep -r "@lru_cache\|@cache\|Cache" --include="*.py" pydantic/networks.py pydantic/v1/tools.py

# Verify cache-related AST nodes in network validation code
python -c "import ast; import sys; tree=ast.parse(open('pydantic/networks.py').read()); print('PASS' if any('cache' in ast.unparse(node).lower() for node in ast.walk(tree)) else 'FAIL')"

# Check for cache-specific test coverage
pytest tests/test_type_hints.py -v -k cache || echo 'No cache-specific tests found'
```

**Accept when:**
- Cache decorators or cache layer implementations are present in network validation modules (pydantic/networks.py, pydantic/v1/tools.py)
- Validation functions for network data types demonstrate caching behavior through decorators, explicit cache checks, or cache utility usage
- Test coverage includes validation of cache behavior for network data type validation scenarios
- Both successful and failed validation outcomes are cached to prevent repeated validation of known-invalid inputs

<enforcement>
Claude Code MUST NOT skip or defer verification of cache layer implementation in external API data validation code. Verification is mandatory before accepting changes to network validation modules.
</enforcement>