# Example Guidance

##### PRD Elements for an AI-Assisted GitHub Copilot CLI Project
When developing an application with GitHub Copilot CLI, your PRD should bridge the gap between human intent and AI execution. It must be precise, structured, and executable so Copilot can follow it without ambiguity Medium.

1. **Project Overview & Goals**
**Purpose**: Clear statement of what the app does and why it’s needed.
**Success criteria**: Measurable outcomes (e.g., “reduce manual CLI commands by 50%”).
**Target audience**: Who will use it (developers, ops, etc.).

2. **Scope & Boundaries**
**In-scope**: Features, integrations, and workflows Copilot will handle.
**Out-of-scope**: What will be excluded to prevent scope creep.
**Dependencies**: External tools, APIs, or services required.

3. **Workflow & Phases**
Break into testable, dependency-ordered phases:
**Setup** – Initialize repo, install tools, configure Copilot CLI.
**Planning** – Use /plan to outline steps; document expected outputs.
**Implementation** – Generate and review diffs; commit changes.
**Testing** – Write and run unit/integration tests.
**Review & Merge** – Follow PR templates and team review rules The GitHub Blog+1.

4. **Technical Specifications**
**Architecture**: Tech stack, frameworks, and design patterns.
**Code style & conventions**: Enforce via .github/copilot-instructions.md GitHub Docs.
**Build/test commands**: e.g., npm run build, npm run test, npm run lint:fix.
**CI/CD rules**: Branch policies, merge strategies, release notes format.

5. **Copilot CLI Configuration**
**Custom instructions file**: .github/copilot-instructions.md with build, test, and style rules GitHub Docs.
**Skills**: .github/skills/ folder for on-demand playbooks (e.g., “generate tests” or “refactor slice”) dxrf.com.
**Prompt files**: .github/prompts/ for reusable scenarios DEV Community.
**Context helpers**: docs/*.spec.md for specs, docs/*.context.md for project memory DEV Community.

6. **Example Use Case**
If building a CLI calculator:
Intent: “Create a Node.js CLI calculator with +, -, *, /, and unit tests.”
Plan: /plan → generate files → add tests → review → commit.
Skills: “implement-from-spec” to auto-generate code from a spec file.

7. **Review & Quality Assurance**
**PR templates**: Required fields (feature name, description, breaking changes).
**Security rules**: How Copilot handles sensitive data.
**Versioning**: Conventional commits, changelog format.

8. **Appendix**
**Glossary**: Definitions of terms.
**Links**: Related docs, GitHub Copilot CLI guides.
**Contact**: PM/owner for clarifications.
