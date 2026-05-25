# Adopt Integration Testing Strategy with Core Metadata Validation: Integration Tests Verify

These rules are ALWAYS ACTIVE for all integration testing implementations across the codebase. All test suites that validate component interactions MUST follow these integration testing patterns.

### Rules

- **R-INT-001** MUST: Integration tests MUST verify that configuration changes propagate correctly through the system and affect dependent components as expected.

### Verify

```bash
# Check for integration test markers or dedicated integration test directories
grep -r "@pytest.mark.integration" tests/ || grep -r "test_integration" tests/

# Count integration test files
find . -path '*/tests/integration/*' -name 'test_*.py' | wc -l

# Collect integration tests via pytest
pytest --collect-only -m integration 2>/dev/null | grep -c '<Module' || echo '0'
```

**Accept when:**
- Integration test markers or dedicated integration test directories are present in the codebase
- At least one integration test exists for each module in _internal/ that handles metadata or configuration
- CI pipeline includes a stage that runs integration tests separately from unit tests
- Test coverage reports show integration test coverage for cross-module interactions

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration tests are mandatory for all cross-module interactions in _internal packages and configuration handling. Violations block PR merges until tests are added.
</enforcement>