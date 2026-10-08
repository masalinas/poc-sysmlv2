# Description

PoC about use sysml skill with Openspec and antigravity agent

## Despendencies

We must install some dependencies

- **STEP01**: Install openjdk21
- **STEP02**: Clone the sysml specification
```shell
git clone --depth 1 https://github.com/Systems-Modeling/SysML-v2-Pilot-Implementation.git
```

- **STEP03**: Clone the sysmlv2-validator to be used with the skill sysmlv2-skill
```shell
git clone https://github.com/DeciSym/sysmlv2-validator.git
```

- **STEP04**: Compile the java validator from clone repo
```shell
mvn -Psetup-dependency initialize
mvn package
```

- **STEP05**: create a soft link for the script validate-sysml to execute the recent compiled validator
sudo ln -s /home/miguel/tools/sysmlv2-validator/validate-sysml /usr/local/bin/validate-sysml

- **STEP06**
Create a sample sysml file to be validated with the tool

```text
package Prueba {
    private import ScalarValues::*;
    part def Vehiculo {
        attribute masa : Real;
    }
}
```

- **STEP07**: Check and validate the sample sysml file
```java
validate-sysml ./sample.sysml
```

- **STEP08**: create the working folder and initialize openspec for the PoC
```shell
mkdir poc-sysmlv2
cd poc-sysmlv2
openspec init
```

- **STEP09**: install sysmlv2-skill inside working folder
Remove manually any .git inside the skill

```shell
cd .agents/skills
git clone git clone https://github.com/DeciSym/sysmlv2-skill.git
```

- **STEP09**: 

```shell
/openspec-explore 

Use the sysmlv2-skill to model a simple vehicle braking system in SysML v2 textual notation and save it as braking.sysml in this workspace.
  
  Requirements:
  - A package called BrakingSystem
  - Attribute definitions for Mass and Pressure
  - Part definitions: BrakePedal, BrakeCaliper and Vehicle
  - Vehicle is composed of one BrakePedal and four BrakeCaliper
  - Vehicle has a mass attribute
  - A requirement definition MaxStoppingDistance with a doc comment
  
  After writing the file, run validate-sysml on it. If it reports errors, fix them and validate again until it passes.
  
  At the end, tell me which skill files you consulted and show the final validation output.

  A simple vehicle braking system model has been created in braking.sysml following the SysML v2 textual notation standards from the sysmlv2-skill.                                                             
                                                                                                                                                                                                                
  ### Model Contents (braking.sysml)                                                                                                                                                                            
                                                                                                                                                                                                                
    package BrakingSystem {                                                                                                                                                                                     
        private import ScalarValues::*;
  
        // Attribute definitions
        attribute def Mass :> Real;
        attribute def Pressure :> Real;
  
        // Part definitions
        part def BrakePedal;
        part def BrakeCaliper;
  
        part def Vehicle {
            attribute mass : Mass;
            part brakePedal : BrakePedal[1];
            part brakeCalipers : BrakeCaliper[4];
        }
  
        // Requirement definitions
        requirement def MaxStoppingDistance {
            doc /* The vehicle stopping distance shall not exceed the specified threshold from reference speed under emergency braking conditions. */
        }
    }
  ──────
  ### Skill Files Consulted
  
  1. SKILL.md – Validation workflow instructions and core rules (mandatory visibility modifiers for imports, specialization requirements for attribute defs, importing ScalarValues).
  2. SYNTAX.md – Syntax reference for package declarations, attribute definitions, composition multiplicity ([1] / [4]), doc comments, and requirement definitions.
  3. ERRORS.md – Error catalog and troubleshooting guidance for KerML / SysML v2 parser rules.
  4. PATTERNS.md – Structural composition, attribute specialization patterns, and requirement specification conventions.
  ──────
  ### Final Validation Output
  
  Running validate-sysml braking.sysml:
  
    $ validate-sysml braking.sysml
    Reading .../Domain Libraries/Quantities and Units/ISQMechanics.sysml...
    Reading .../Domain Libraries/Quantities and Units/ISQAcoustics.sysml...
    Reading .../Domain Libraries/Quantities and Units/ISQLight.sysml...
    Reading .../Domain Libraries/Quantities and Units/ISQElectromagnetism.sysml...
    Reading .../Domain Libraries/Geometry/ShapeItems.sysml...
    Reading .../Domain Libraries/Geometry/SpatialItems.sysml...
    Reading .../Domain Libraries/Metadata/ImageMetadata.sysml...
    Reading .../Domain Libraries/Metadata/RiskMetadata.sysml...
    Reading .../Domain Libraries/Metadata/ParametersOfInterestMetadata.sysml...
    Reading .../Domain Libraries/Metadata/ModelingMetadata.sysml...
    Reading .../Domain Libraries/Requirement Derivation/DerivationConnections.sysml...
    Reading .../Domain Libraries/Requirement Derivation/RequirementDerivation.sysml...
    Reading .../Domain Libraries/Cause and Effect/CauseAndEffect.sysml...
    Reading .../Domain Libraries/Cause and Effect/CausationConnections.sysml...
    Reading .../Domain Libraries/Analysis/TradeStudies.sysml...
    Reading .../Domain Libraries/Analysis/StateSpaceRepresentation.sysml...
    Reading .../Domain Libraries/Analysis/SampledFunctions.sysml...
    Reading .../Domain Libraries/Analysis/AnalysisTooling.sysml...
  
    Exit code: 0
  
  The model passed validation with zero errors.
```
