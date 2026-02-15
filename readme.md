# Benchmark Polytope Import Suite for volesti

## Overview
volesti is a C++ library that helps compute the volume of convex polytopes and also sample points from them.But when we try to use the realworld benchmark datasets like 
Netlib LP problems or metabolic flux models they are not directly compatible with volesti’s internal Hpolytope format. So before using them we need some extra conversion steps.
In this project, I want to build a simple and structured import layer that can take these benchmark datasets and convert them correctly into validated HPolytope objects
so they can be used directly inside volesti without manual adjustments.

## Current volesti Input Pipeline
Currently, volesti supports:

- Programmatic construction: HPolytope(d, A, b)
- cdd-style .ine files via read_pointset()

Internally, polytopes are represented as: Ax ≤ b

## Identified Limitations

- There has no structural validation in .ine constructor.
- Implicit dimension assumption.
- There has not any direct support for LP bounds.
- No metadata handling
- No reusable import abstraction layer

## Proposed Solution

Introduce a modular import layer:

- `PolytopeData` — canonical intermediate representation
- `LPImporter` — Netlib MPS → (A, b)
- `MetabolicImporter` — Stoichiometric model → (A, b)
- `PolytopeValidator` —  checks structural and feasibility

All pipelines end in: HPolytope(d, A, b)

## Roadmap (a simple approcing way from my side)
- 1.First clearly define a clean and consistent internal representation for polytopes so everything follows the same structure.
- 2.Then build validation tools to check if the imported data is correct, well-formed and actually represents a feasible polytope.
- 3.Next create a converter that can take LP benchmark datasets and transform them into the required (A, b) format.
- 4.Build another converter for metabolic models, so stoichiometric data can also be transformed properly.
- 5.Finally add clear examples and small benchmarks so others can easily reproduce results and understand how the pipeline works.


