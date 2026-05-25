# Adopt Integration Testing Strategy with Core Metadata Validation: Integration Tests Organized

These rules are ALWAYS ACTIVE for all integration testing implementations across the codebase, particularly for test suites that validate component interactions, cross-module dependencies, metadata handling, and configuration propagation.

### Rules

- **R-INT-001** SHOULD: Integration tests SHOULD be organized separately from unit tests to enable independent execution and clear distinction of test scope.

### Verify

```bash
# Check for integration test markers or dedicated directories
grep -r "@pytest.mark.integration" tests/ || grep -r "test_integration" tests/

# Count integration test files in dedicated directory
find . -path '*/tests/integration/*' -name 'test_*.py' | wc -l

# Collect integration tests via pytest
pytest --collect-only -m integration 2>/dev/null | grep -c '<Module' || echo '0'
```

**Accept when:**
- Integration test markers (e.g., `@pytest.mark.integration`) or dedicated integration test directories are present in the codebase
- At least one integration test exists for each module in `_internal/` that handles metadata or configuration
- CI pipeline includes a stage that runs integration tests separately from unit tests
- Test coverage reports show integration test coverage for cross-module interactions

<enforcement>
Claude Code MUST NOT skip or defer verification of integration test organization. All new _internal modules handling metadata or configuration MUST include corresponding integration tests before merge.
</enforcement>