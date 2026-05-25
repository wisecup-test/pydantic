# Adopt Integration Testing Strategy with Core Metadata Validation: Integration Test Suites

These rules are ALWAYS ACTIVE for all integration testing implementations across the codebase, particularly for test suites that validate component interactions, metadata consistency, and configuration propagation across module boundaries.

### Rules

- **R-INT-001** MUST: Integration test suites MUST validate metadata consistency across internal module boundaries, particularly for core functionality in _internal packages.
- **R-INT-002** MUST: All _internal package modules that handle metadata or configuration MUST have corresponding integration tests validating cross-module interactions.
- **R-INT-003** MUST: Integration tests MUST use pytest markers (e.g., @pytest.mark.integration) to enable selective test execution separate from unit tests.
- **R-INT-004** SHOULD: Integration test suites SHOULD be organized in a tests/integration directory structure that mirrors the main codebase structure.
- **R-INT-005** SHOULD: Reusable test fixtures for common integration scenarios (e.g., configuration setup, metadata initialization) SHOULD be developed to reduce boilerplate.
- **R-INT-006** SHOULD: Integration test patterns and examples SHOULD be documented in the project's testing guide, including clear guidance on when to write integration vs unit tests.

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
Claude Code MUST NOT skip or defer verification of integration test presence and markers. All new _internal modules handling metadata or configuration MUST include integration tests before merge.
</enforcement>