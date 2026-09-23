# MISSING sentinel unification across schema generation and serialization: Core Schema Definitions Serialization Layers Represent

Status: proposed
Date: 2026-09-23
Deciders: AI (signal conversion)

## Context

- In Pydantic schema generation and core serialization, representing absent or unset attributes historically relied on omitting dictionary keys or overloading None. This created ambiguity in union branches, discriminated type resolutions, and model serialization when handling exclude_unset configurations.
- To resolve ambiguity across both Python schema generation and Rust core serialization routines, a unified MISSING sentinel is introduced to represent omitted attributes distinctly from values that are explicitly null.
- The change introduces the MISSING sentinel into core_schema definitions, union constraint pushdown in schema generation, and unset attribute patching during model serialization.

## Problem Statement

Overloading None or omitting keys to represent absent attributes causes ambiguity between omitted values and explicit null values across Python schema generation and core serialization layers.

## Decision

1. MUST: Core schema definitions and serialization layers MUST represent absent or omitted attributes using the explicit MISSING sentinel rather than None.

## Policy Block

- MUST Core schema definitions and serialization layers MUST represent absent or omitted attributes using the explicit MISSING sentinel rather than None.

In scope:
- Core schema definitions
- Internal schema generation and gathering logic
- Type serializer implementations handling model attribute serialization and unset filtering
- JSON schema emission layers

Out of scope:
- External user-facing payload inputs prior to model parsing
- Unrelated third-party plugin schemas

## Rationale

- Using a dedicated MISSING sentinel provides a singular, unambiguous representation for omitted versus null attributes across Python and Rust serialization boundaries.
- Union validation and discriminated type resolution require explicit knowledge of absent fields to push down constraints correctly without false matches against None.
- Enforcing strict boundaries against sentinel leakage ensures internal schema semantics do not corrupt external consumer payloads or JSON schema definitions.

## Consequences

Positive:
- Consistent semantics between Python schema generation and Rust serialization for omitted fields.
- Elimination of ambiguity between null and omitted values during union branch evaluation.
- Reliable handling of exclude_unset during model serialization.

Negative:
- Requires explicit sentinel handling across all schema gatherers, serializers, and JSON schema translators.
- Introduces risk of leaking internal sentinel representations if serialization output checks are missed.

## Alternatives

- Overload None or omit dictionary keys without a dedicated core sentinel representation (rejected)
  Rejected because: Creates ambiguity between explicitly null attributes and omitted or unset attributes, leading to incorrect resolution in union branches and discriminated types.

## Risks

- The internal MISSING sentinel object could leak into serialized JSON payloads or generated JSON schema outputs.
  Mitigation: Ensure serialization and JSON schema generation layers filter or convert MISSING before emitting final outputs, backed by automated tests.
  Owner: Architecture Review

## Implementation Notes

- Propagate the MISSING symbol from the core schema module into internal schema generation and gather modules.
- Implement model serializer handling in Rust type serializers to treat MISSING appropriately under exclude_unset conditions.
- Verify JSON schema generators suppress or cleanly map absent fields to avoid emitting sentinel markers.

## Continuation Context


Verify commands:
- Discover and execute test suites targeting union branch resolution, missing sentinel behavior, and model serialization
- Discover and execute core serializer verification tests across Python and native extensions

Accept when:
- All schema generation tests pass without substituting None for missing attributes
- Model serialization with exclude_unset correctly handles MISSING without exposing the sentinel object
- JSON schema export contains standard JSON types without sentinel leakage

## Enforcement

- Verified by: Automated test suites covering union evaluation, missing sentinels, and model serialization
- Verified by: Static analysis and code review checks across schema generation and serializer implementations
- Violation handling: Pull requests leaking MISSING to downstream consumers or using None for absent fields must be rejected at review
- Violation handling: Test failures for sentinel leakage or union branch mismatch block CI merges
- Exception process: Exceptions require submission of an architectural review request documenting why the MISSING sentinel cannot be used in a specific schema layer

## References

- file:pydantic-core/python/pydantic_core/core_schema.py
- file:pydantic-core/src/serializers/type_serializers/model.rs
- file:pydantic/_internal/_generate_schema.py
- file:pydantic/_internal/_schema_gather.py
- file:pydantic/json_schema.py
- file:tests/test_missing_sentinel.py
- file:tests/types/test_union.py
- commit:94dd544cc0ba0692801f429dd64124db76a4537d
- commit:0e4be5e64777542a06eebbfa298e4e6e5884593e
- commit:2a806ad09b984fcc43568191aba5d965350995a0
- commit:597fa692eaaa90132f683e88ca994f28e185463b
- pr:#12908
- pr:#12905
- pr:#13048
- pr:#13135