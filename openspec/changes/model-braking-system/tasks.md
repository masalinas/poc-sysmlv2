# Tasks

## 1. SysML v2 Model Definition

- [x] 1.1 Create `braking.sysml` with `BrakingSystem` package and `private import ScalarValues::*;`, and verify package structure
- [x] 1.2 Define attribute definitions `Mass` and `Pressure` specializing `Real`, and verify type declarations in `braking.sysml`
- [x] 1.3 Define part definitions `BrakePedal`, `BrakeCaliper`, and `Vehicle` with component composition (`brakePedal : BrakePedal[1]`, `brakeCalipers : BrakeCaliper[4]`, and `mass : Mass`), and verify composition syntax
- [x] 1.4 Add `MaxStoppingDistance` requirement definition with descriptive doc comment, and verify documentation comment formatting

## 2. Metamodel Validation

- [x] 2.1 Run `validate-sysml braking.sysml` using the SysML v2 Pilot Implementation and verify command exits with code 0 and zero error diagnostics
