# Standardize Set and FrozenSet Serialization in Public API Responses: Implementations Sort Serialized

These rules are ALWAYS ACTIVE for all public-facing REST API endpoints, GraphQL API responses, webhook payloads, event streams, and any code that serializes Python set or frozenset types for external consumption.

### Rules

- **R-SET-001** MAY: Implementations MAY sort serialized set elements for deterministic output if performance requirements permit.
- **R-SET-002** MUST: All set and frozenset fields in API responses serialize to valid JSON arrays.
- **R-SET-003** MUST: Serialization behavior must be identical between Rust core (pydantic-core) and Python validator implementations.
- **R-SET-004** MUST: OpenAPI schemas must correctly represent set/frozenset types with array type and uniqueItems constraint.
- **R-SET-005** SHOULD: Implementations should document in API specifications that array order is not guaranteed and may vary between responses.
- **R-SET-006** SHOULD: Implementations should implement server-side validation to reject arrays with duplicates when deserializing to sets.

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
- Integration tests verify set serialization behavior across different API endpoints and response types
- CI pipeline validates serialization output format matches expected array structure

<enforcement>
Claude Code MUST NOT skip or defer verification. All set and frozenset serialization must be validated against the rules and acceptance criteria before code is approved.
</enforcement>