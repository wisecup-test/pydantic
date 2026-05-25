# Standardize Set and FrozenSet Serialization in Public API Responses: Documentation Explicitly Note

These rules are ALWAYS ACTIVE for all public-facing REST API endpoints, GraphQL API responses, webhook payloads, event streams, API response schemas, and OpenAPI specifications that expose set or frozenset types.

### Rules

- **R-SET-001** SHOULD: API documentation SHOULD explicitly note that set/frozenset types are serialized as arrays and order is not guaranteed.
- **R-SET-002** MUST: All set and frozenset fields in API responses MUST serialize to valid JSON arrays.
- **R-SET-003** MUST: Serialization behavior MUST be identical between Rust core (pydantic-core) and Python validator implementations.
- **R-SET-004** MUST: OpenAPI schemas MUST correctly represent set/frozenset types with array type and uniqueItems constraint.
- **R-SET-005** SHOULD: Integration tests SHOULD verify set serialization behavior across different API endpoints and response types.
- **R-SET-006** SHOULD: Configuration flags SHOULD be considered to enable deterministic sorting for testing environments while keeping production unsorted for performance.

### Verify

```bash
# Verify set serialization in pydantic-core and validators
grep -r 'set.*serializ' pydantic-core/src/serializers/ pydantic/validators.py | grep -v test

# Test set serialization output format
python -c "from pydantic import BaseModel; from typing import Set; class M(BaseModel): s: Set[int]; import json; print(json.loads(M(s={3,1,2}).json())['s'])"

# Verify type serializers for set and frozenset in Rust
rg 'type_serializers/(set|frozenset)' --type rust
```

**Accept when:**
- All set and frozenset fields in API responses serialize to valid JSON arrays
- Serialization behavior is identical between Rust core and Python validator implementations
- OpenAPI schemas correctly represent set/frozenset types with array type and uniqueItems constraint
- Integration tests validate serialization output format across all endpoints
- Documentation explicitly states that set/frozenset order is not guaranteed

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST fail if serialization tests detect non-array output for set/frozenset types. API gateway validation MUST reject responses that don't match OpenAPI schema. Runtime monitoring MUST alert on serialization errors or type mismatches.
</enforcement>