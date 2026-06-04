# SmartGuardian — Requirements and Modeling Repository

SmartGuardian is a software engineering course project for requirements analysis, system modeling, architecture documentation, and validation planning. The repository is intentionally documentation-focused; it does not contain application source code.

## Project Summary

SmartGuardian is a smart home monitoring and security platform concept for infrastructure-constrained contexts where power instability, intermittent connectivity, and safety risks are common. The system focuses on simulated IoT devices, centralized monitoring, role-based access, security alerts, and explainable predictive battery warnings.

Start with [PROJECT-OVERVIEW.md](PROJECT-OVERVIEW.md) for the short system summary.

## Main Artifact Map

| Folder | Purpose | Key artifacts |
| --- | --- | --- |
| `01-vision-and-scope` | Explains the problem, stakeholders, goals, and system boundary | Problem statement, stakeholders, business goals, context diagram |
| `02-requirements` | Defines and validates what the system must satisfy | Functional requirements, non-functional requirements, validation notes, traceability matrix |
| `03-use-case-model` | Models user goals and actor-system interactions | Use case diagram, briefs, fully dressed use cases, misuse cases |
| `04-analysis-model` | Translates requirements into analysis-level behavior and concepts | Domain model, system sequence diagrams, operation contracts |
| `05-design-model` | Documents object-oriented design and responsibility assignment | Class diagram, communication diagrams, activity diagrams, state machine diagrams, GRASP patterns |
| `06-architecture` | Describes high-level structure, deployment, and major decisions | Architectural drivers, component diagram, deployment diagram, security view, technology decisions |
| `07-validation` | Shows how requirements and models are checked | System test scenarios, validation plans, risk analysis, evaluation checklist |
| `08-final-report` | Collects final evaluation and presentation material | Final architecture summary, demo scenarios, presentation outline |

## Recommended Reading Order

1. [PROJECT-OVERVIEW.md](PROJECT-OVERVIEW.md)
2. [01-vision-and-scope/problem-statement.md](01-vision-and-scope/problem-statement.md)
3. [02-requirements/functional-requirements.md](02-requirements/functional-requirements.md)
4. [02-requirements/non-functional-requirements.md](02-requirements/non-functional-requirements.md)
5. [03-use-case-model/use-case-diagram.md](03-use-case-model/use-case-diagram.md)
6. [04-analysis-model/domain-model.md](04-analysis-model/domain-model.md)
7. [04-analysis-model/system-sequence-diagrams.md](04-analysis-model/system-sequence-diagrams.md)
8. [05-design-model/class-diagram.md](05-design-model/class-diagram.md)
9. [05-design-model/grasp-patterns.md](05-design-model/grasp-patterns.md)
10. [06-architecture/component-diagram.md](06-architecture/component-diagram.md)
11. [02-requirements/requirements-traceability-matrix.md](02-requirements/requirements-traceability-matrix.md)
12. [08-final-report/final-architecture-summary.md](08-final-report/final-architecture-summary.md)

## Architecture Process

The architecture documentation follows this sequence:

1. Capture architectural drivers from requirements and constraints.
2. Decompose the system into collaborating subsystems.
3. Document component, deployment, and security views.
4. Evaluate technology decisions against quality attributes.
5. Validate architecture consistency through requirements traceability.

## Quality Gates

- Required modeling artifacts are present and easy to locate.
- Requirements connect to use cases, analysis models, design models, and validation scenarios.
- UML-style diagrams use consistent terminology.
- Architecture decisions and trade-offs are documented.
- Placeholder files are avoided unless they identify a required artifact still pending completion.
