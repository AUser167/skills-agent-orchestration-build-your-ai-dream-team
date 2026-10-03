# Project Pulse agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate this custom agent
team as we plan, design, build, and validate Mona's Project Pulse dashboard.
Each agent uses GPT-6 Luna, as requested for the free-tier setup and reasonable
token consumption.

| Agent | Model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | GPT-6 Luna | Coordinates the specialists, assigns non-overlapping work, integrates the results, and reports validation and handoff. | `.github/agents/orchestrator.agent.md` |
| Planner | GPT-6 Luna | Researches the project and produces an implementation plan with phases, file ownership, dependencies, risks, and validation expectations. | `.github/agents/planner.agent.md` |
| Designer | GPT-6 Luna | Guides the dashboard's usability, accessibility, information hierarchy, responsive layout, and visual design. | `.github/agents/designer.agent.md` |
| Coder | GPT-6 Luna | Implements and validates the assigned dashboard code and supporting runnable-app configuration. | `.github/agents/coder.agent.md` |

The Orchestrator will first ask the Planner for a plan, then delegate visual
design guidance to the Designer and implementation to the Coder with clear
file ownership. Finally, the Orchestrator will review the integrated dashboard
and summarize its validation and handoff.
