# Adoption of DecoratorInfos for Class Decorator Extraction and Descriptor Proxy Resolution: When Class Level Decorators Alter Method

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Context

- Type definitions and model representations require inspecting user-defined decorators, descriptor proxies, and bound functions across inheritance hierarchies and namespace dictionaries.
- Direct, ad-hoc attribute reflection across diverse data modeling abstractions leads to duplicated traversal logic, inconsistent handling of method wrappers, and incomplete extraction of validation and serialization metadata.
- A centralized builder interface is required to extract decorator metadata, manage descriptor shims, and replace wrapped methods uniformly during class synthesis and schema generation.

## Problem Statement

Dispersed and manual parsing of class namespaces for custom decorators across models, dataclasses, and typed dictionaries causes maintenance fragmentation, missed descriptor attributes, and inconsistent binding to class references during schema generation. A unified extraction and assembly routine is necessary to reliably discover, unwrap, and bind decorator configurations without polluting downstream type compilation logic.

## Decision

1. MUST: When class-level decorators alter method invocation semantics, DecoratorInfos.build MUST be invoked with wrapped method replacement enabled during class creation, and with replacement disabled when generating schemas for static type definitions.

## Policy Block

- MUST When class-level decorators alter method invocation semantics, DecoratorInfos.build MUST be invoked with wrapped method replacement enabled during class creation, and with replacement disabled when generating schemas for static type definitions.

In scope:
- Class construction metaclasses and dataclass transformation routines.
- Schema generation pipelines processing decorated data structures.
- Descriptor proxy resolution and decorator metadata extraction.

Out of scope:
- Runtime instance validation routines executing after schema compilation.
- Static type checker plugin implementations.

Exceptions:
- EXC-39-001: A specialized type representation lacks a runtime class dictionary and operates strictly on synthetic type annotations.

## Rationale

- Centralizing decorator extraction via DecoratorInfos.build guarantees that inheritance traversal, descriptor unwrapping, and metadata indexing occur identically across all supported class structures.
- Controlling method wrapping behavior through an explicit replacement parameter prevents unwanted mutation during read-only schema generation while ensuring active wrapping during metaclass execution.
- Abstracting descriptor proxy handling into dedicated containers prevents namespace collision and warns against accidental decorator overrides before runtime execution.

## Consequences

Positive:
- Ensures consistent decorator resolution and validator binding across all model and dataclass variants.
- Eliminates redundant namespace inspection routines across schema generation and class construction subsystems.
- Provides clear visibility into descriptor wrapping and method overrides at class definition time.

Negative:
- Introduces reflection and metadata extraction overhead during class creation.
- Couples class construction and schema compilation modules to the internal decorator representation interface.

## Alternatives

- Direct namespace inspection and manual decorator extraction within each class metaclass and schema builder (rejected)
  Rejected because: Duplicated traversal logic across multiple modules caused inconsistent behavior with descriptor proxies, method unwrap failures, and divergent decorator handling between models and dataclasses.
  When valid: Lightweight architectures where only a single type definition pattern exists without inheritance or descriptor shims.
- Deferred decorator resolution evaluated lazily upon first instance validation (rejected)
  Rejected because: Postponing decorator inspection to instantiation introduces runtime latency penalties and prevents early validation of schema invariants during model definition.
  When valid: Environments prioritizing class definition startup time over validation performance and deterministic schema generation.

## Risks

- Performance degradation during class definition due to repeated base class inspection and descriptor unwrapping.
  Mitigation: Cache intermediate inspection results and unwrap partial functions only when non-callable descriptors are encountered.
  Owner: Core Architecture Team
- Inadvertent method replacement during read-only schema analysis mutating target class attributes.
  Mitigation: Enforce boolean parameter flags that explicitly prohibit method replacement in schema generation pathways.
  Owner: Quality Assurance Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When extracting decorators during class definition, pass the flag to replace wrapped methods to ensure runtime descriptor proxies bind correctly to the constructed class.
- When compiling schemas for classes where method mutation must be avoided, invoke the builder with method replacement disabled.

## Continuation Context


Verify commands:
- Discover the project test runner configuration from the repository manifest and execute the test suite targeting decorator and model construction subsystems.
- Locate the linting and static analysis configurations in the project workspace and run the static verification checks across internal modules.

Accept when:
- All decorator extraction and model construction test suites execute without failures or unhandled descriptor warnings.
- Static type analysis confirms that all call sites conform to the builder interface parameters and expected return types.

## Enforcement

- Verified by: Automated continuous integration test suites executed against all pull requests.
- Verified by: Static type analysis and linting checks enforcing builder usage.
- Verified by: Mandatory architectural peer review of changes to class construction and decorator compilation modules.
- Violation handling: Code submissions containing manual attribute inspection or bypassing the centralized builder will be blocked during review.
- Violation handling: Continuous integration failure on test regressions or type mismatches in decorator extraction.
- Exception process: Submit an architectural exception request detailing the constraints that necessitate custom namespace extraction, subject to approval by the core maintainers.