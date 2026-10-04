# Agent team

I will use the following custom agents to build Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the dashboard work, delegates scoped tasks to specialists, manages dependencies and phases, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, then creates an implementation plan with ordered steps, file assignments, dependencies, edge cases, and validation expectations. | `.github/agents/planner.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Designs the Project Pulse dashboard experience, including usability, accessibility, information hierarchy, responsive behavior, and polished visual styling. | `.github/agents/designer.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements assigned dashboard code and runnable-app support, following repository patterns and validating changes. | `.github/agents/coder.agent.md` |

I am using GitHub Copilot CLI in a Codespace to orchestrate the work.
