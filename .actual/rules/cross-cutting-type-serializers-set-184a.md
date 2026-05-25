# Standardize Set and FrozenSet Serialization in Public API Responses: Type Serializers Set

These rules are ALWAYS ACTIVE for all public-facing REST API endpoints, GraphQL API responses, webhook payloads, event streams, and any code that serializes Python set or frozenset types through the public API surface.

### Rules

- **R-SERIALIZERS-SET-001** MUST: Type serializers for set and frozenset MUST handle empty collections without errors.

### Verify

```bash
# Verify set serialization in pydantic-core
grep -r 'set.*serializ' pydantic-core/src/serializers/ pydantic/validators.py | grep -v test

# Test empty set serialization behavior
python -c "from pydantic import BaseModel; from typing import Set; class M(BaseModel): s: Set[int]; import json; print(json.loads(M(s={3,1,2}).json())['s'])"

# Verify type serializers for set and frozenset exist
rg 'type_serializers/(set|frozenset)' --type rust
```

**Accept when:**
- All set and frozenset fields in API responses serialize to valid JSON arrays
- Serialization behavior is identical between Rust core and Python validator implementations
- OpenAPI schemas correctly represent set/frozenset types with array type and uniqueItems constraint
- Empty sets serialize without raising exceptions

<enforcement>
Claude Code MUST NOT skip or defer verification. All set and frozenset serialization must be tested before merging changes to public API endpoints.
</enforcement>