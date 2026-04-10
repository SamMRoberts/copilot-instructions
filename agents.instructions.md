---
description: 'Use these guidelines when generating or updating custom Copilot agent files.'
applyTo: .github/agents/**
---
# Guardrail-TDD Agent Authoring Guidelines

## Purpose
- Use agent files to define reusable Copilot personas with clear tool boundaries.
- Keep agents focused on one primary role or workflow.

## File and naming conventions
- Store workspace agents in `.github/agents/`.
- Use the `.agent.md` extension.
- Choose short, descriptive names that match the role.
- Keep one agent persona per file; split unrelated responsibilities into separate agents.

## Required structure
- Include YAML frontmatter at the top of each agent file.
- Include at least these fields when applicable:
	- `name`
	- `description`
	- `tools`
	- `user-invocable`
	- `disable-model-invocation`
- Optionally include:
	- `argument-hint`
	- `handoffs`
	- `model`
	- `agents`
- Prefer explicit `user-invocable` and `disable-model-invocation` flags instead of deprecated `infer`.
- If `agents` is used, ensure the `agent` tool is included in `tools`.
- Keep frontmatter valid YAML and match field names exactly.

## Tool selection rules
- Follow least privilege: grant only tools needed for the role.
- Use read-focused tools for planning, review, and analysis agents.
- Add edit or execute tools only when implementation or test execution is explicitly required.
- Avoid broad tool lists by default.
- Ensure instructions in the body are achievable with the declared tools.
- Remember unavailable tools are ignored at runtime, so avoid depending on optional tooling without fallback guidance.

## Handoff guidance
- Use `handoffs` to model multi-step workflows when useful.
- Keep handoff labels action-oriented and concise.
- Ensure handoff prompts are explicit about next-step intent.
- Set `handoffs.send: true` only when automatic prompt submission is clearly intended.

## Subagents
- A subagent is another agent invoked by the current agent to complete a specialized step.
- Use subagents to split complex workflows into focused roles (for example: plan -> implement -> review).
- Configure allowed subagents with the `agents` frontmatter field.
- If you set `agents`, include the `agent` tool in `tools` so subagent invocation is possible.
- Use `user-invocable: false` for agents meant to run only as subagents.
- Use `disable-model-invocation: true` when an agent should never be invoked as a subagent by others.
- Keep subagent boundaries explicit so each agent has a narrow, predictable responsibility.

## Instruction body quality
- State the agent goal first.
- Define boundaries, expected behavior, and output expectations.
- Prefer concrete steps over generic advice.
- Avoid contradictory guidance between frontmatter and body instructions.
- Keep prompts deterministic and role-specific so behavior is consistent across sessions.

## Change hygiene
- Keep edits minimal and focused to the requested agent behavior.
- Preserve working frontmatter and only adjust fields needed for the requested change.
- Validate YAML formatting and field names after edits.

## Reference
- VS Code Custom Agents documentation: https://code.visualstudio.com/docs/copilot/customization/custom-agents
