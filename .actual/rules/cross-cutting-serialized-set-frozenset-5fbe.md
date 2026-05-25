# Standardize Set and FrozenSet Serialization in Public API Responses: Serialized Set Frozenset

These rules are ALWAYS ACTIVE for all public-facing REST API endpoints, GraphQL API responses, webhook payloads, event streams, and API response schemas that expose Python set or frozenset types.

### Rules

- **R-SERIALIZED-SET-001** SHOULD: Serialized set and frozenset arrays SHOULD maintain element uniqueness constraints during validation.

### Verify

```bash
# Verify set serialization in pydantic-core and validators
grep -r 'set.*serializ' pydantic-core/src/serializers/ pydantic/validators.py | grep -v test

# Test set serialization behavior
python -c "from pydantic import BaseModel; from typing import Set; class M(BaseModel): s: Set[int]; import json; print(json.loads(M(s={3,1,2}).json())['s'])"

# Verify Rust type serializers for set and frozenset
rg 'type_serializers/(set|frozenset)' --type rust
```

**Accept when:**
- All set and frozenset fields in API responses serialize to valid JSON arrays
- Serialization behavior is identical between Rust core and Python validator implementations
- OpenAPI schemas correctly represent set/frozenset types with array type and uniqueItems constraint
- Integration tests verify set serialization behavior across different API endpoints and response types

<enforcement>
Claude Code MUST NOT skip or defer verification. All set and frozenset serialization must be validated before merging changes to public API endpoints.
</enforcement>