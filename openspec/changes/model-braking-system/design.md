# Design

## Context

See `proposal.md` for motivation. The repository focuses on SysML v2 systems modeling using textual notation, following the guidance from `sysmlv2-skill` and validated via the OMG SysML v2 Pilot Implementation command-line tool (`validate-sysml`).

SysML v2 imposes strict KerML metamodel rules:
- Imports of standard libraries require explicit visibility modifiers (`private import ScalarValues::*;`).
- Attribute definitions must specialize standard data values (`:> Real`).
- Part definitions define components that can be composed with multiplicity notation.

## Goals / Non-Goals

**Goals:**
- Provide a clean, compliant SysML v2 model file in `braking.sysml`.
- Model component decomposition: `Vehicle` composing one `BrakePedal` and four `BrakeCaliper` parts.
- Model physical attributes: `Mass` and `Pressure` specializing `Real`.
- Define system requirement `MaxStoppingDistance` with descriptive documentation comments.
- Ensure 100% compliance with `validate-sysml`.

**Non-Goals:**
- Modeling detailed hydraulic fluid item flows or port connections (deferred to future changes).
- Dynamic mathematical simulation or behavioral state machine execution.

## Decisions

### 1. Unified Package Layout
Wrap all definitions within a single package `BrakingSystem` in `braking.sysml`.
- **Rationale**: Keeps the model cohesive, self-contained, and easily imported into visual tools (such as Eclipse SysON) or validated via CLI.
- **Alternatives Considered**: Splitting into separate files per component definition; rejected as unnecessary overhead for a preliminary subsystem model.

### 2. Explicit Attribute Specialization
Define `attribute def Mass :> Real;` and `attribute def Pressure :> Real;` after `private import ScalarValues::*;`.
- **Rationale**: In KerML/SysML v2, attribute definitions cannot stand alone without specializing a primitive scalar data type.
- **Alternatives Considered**: Using unspecialized attributes; rejected as invalid under the SysML v2 metamodel.

### 3. Multiplicity for Part Composition
Declare parts in `Vehicle` as `part brakePedal : BrakePedal[1];` and `part brakeCalipers : BrakeCaliper[4];`.
- **Rationale**: Directly expresses automotive wheel brake caliper configuration and driver pedal interface with standard SysML v2 cardinality.
- **Alternatives Considered**: Defining individual calipers (`frontLeft`, `frontRight`, etc.); rejected for simplicity in the initial architectural baseline.

### 4. Doc Comment Requirement Specification
Use `doc /* ... */` inside `requirement def MaxStoppingDistance`.
- **Rationale**: Standard SysML v2 textual syntax for requirements documentation.

## Risks / Trade-offs

- **[Risk] Missing port interfaces limits interaction analysis** → Mitigation: Keep the initial model focused on structural composition; add ports (`in/out` fluid/mechanical pressure) in an upcoming iteration.
- **[Risk] Validator dependency requires Java and Pilot Implementation environment** → Mitigation: Document validation command and integrate checks directly in CI/developer workflows.
