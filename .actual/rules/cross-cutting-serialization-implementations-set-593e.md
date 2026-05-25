# Standardize Set and FrozenSet Serialization in Public API Responses: Serialization Implementations Set

These rules are ALWAYS ACTIVE for all public-facing API endpoints, GraphQL responses, webhook payloads, and any code that serializes Python set or frozenset types for external consumption.

### Rules

- **R-SER-001** MUST: Serialization implementations for set and frozenset types MUST be consistent across all serialization layers (Rust core and Python validators).
- **R-SER-002** MUST: All set and frozenset fields in API responses MUST serialize to valid JSON arrays.
- **R-SER-003** MUST: OpenAPI schemas MUST correctly represent set/frozenset types with array type and uniqueItems constraint.
- **R-SER-004** SHOULD: Implement deterministic sorting for testing environments to ensure reproducible test fixtures.
- **R-SER-005** SHOULD: Document in API specifications that array order is not guaranteed and may vary between responses.

### Verify

```bash
# Verify set/frozenset serialization patterns in codebase
grep -r 'set.*serializ' pydantic-core/src/serializers/ pydantic/validators.py | grep -v test

# Test serialization output format
python -c "from pydantic import BaseModel; from typing import Set; class M(BaseModel): s: Set[int]; import json; print(json.loads(M(s={3,1,2}).json())['s'])"

# Verify Rust type serializers for set/frozenset
rg 'type_serializers/(set|frozenset)' --type rust
```

**Accept when:**
- All set and frozenset fields in API responses serialize to valid JSON arrays
- Serialization behavior is identical between Rust core and Python validator implementations
- OpenAPI schemas correctly represent set/frozenset types with array type and uniqueItems constraint
- Integration tests verify set serialization behavior across different API endpoints and response types
- CI pipeline validates serialization output format matches expected array structure

<enforcement>
Claude Code MUST NOT skip or defer verification. All serialization implementations for set and frozenset types must be validated against the verify commands before acceptance. Violations detected by CI pipeline must be resolved before deployment.
</enforcement>