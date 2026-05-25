# Standardize Set and FrozenSet Serialization in Public API Responses: Set Frozenset Types

These rules are ALWAYS ACTIVE for all public API endpoints, GraphQL responses, webhook payloads, and event streams that expose Python set or frozenset types.

### Rules

- **R-SETFROZEN-001** MUST: All set and frozenset types exposed through public API endpoints MUST be serialized to JSON arrays.
- **R-SETFROZEN-002** MUST: Serialization behavior MUST be identical between Rust core (pydantic-core) and Python validator implementations.
- **R-SETFROZEN-003** MUST: OpenAPI schemas MUST correctly represent set/frozenset types as arrays with the `uniqueItems` constraint.
- **R-SETFROZEN-004** SHOULD: Implement deterministic sorting for testing environments while keeping production unsorted for performance.
- **R-SETFROZEN-005** SHOULD: Document in API specifications that array order is not guaranteed and may vary between responses.
- **R-SETFROZEN-006** MAY: Apply exception EXC-001 only to legacy API versions (v1.x) with documented set serialization behavior different from this standard.

### Verify

```bash
# Verify set/frozenset serialization patterns in codebase
grep -r 'set.*serializ' pydantic-core/src/serializers/ pydantic/validators.py | grep -v test

# Test pydantic serialization output format
python -c "from pydantic import BaseModel; from typing import Set; class M(BaseModel): s: Set[int]; import json; print(json.loads(M(s={3,1,2}).json())['s'])"

# Verify Rust type serializers for set and frozenset
rg 'type_serializers/(set|frozenset)' --type rust
```

**Accept when:**
- All set and frozenset fields in API responses serialize to valid JSON arrays
- Serialization behavior is identical between Rust core and Python validator implementations
- OpenAPI schemas correctly represent set/frozenset types with array type and uniqueItems constraint
- Integration tests verify set serialization behavior across different API endpoints and response types
- CI pipeline validates serialization output format matches expected array structure

<enforcement>
Claude Code MUST NOT skip or defer verification. All set and frozenset serialization MUST be validated against the verify commands before accepting any implementation. Violations detected by CI pipeline MUST result in build failure.
</enforcement>