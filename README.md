# 1MSG for agents

Agent instructions for building and operating WhatsApp integrations through 1MSG. The repository separates reusable skills from future client-specific plugins, so a skill can be used without a particular editor or transport.

## Available skill

[1msg-integration](skills/1msg-integration/SKILL.md) guides an agent from the requested integration outcome through verified messaging, inbound events, reliability, tool selection, and troubleshooting. It includes seven references loaded only when relevant to the task.

The skill is written in English and adapted from the approved 1MSG integration guide. It contains decision rules and workflows rather than copied API examples, code, or pricing tables. Request syntax belongs in the maintained API and client documentation.

## Use the skill

Add the entire `skills/1msg-integration` directory to a skill-capable agent's configured skill location, preserving its `references` and optional `agents` metadata directories. Follow that agent host's current installation and discovery instructions. The entry point is `SKILL.md`; the references are not independently installable skills.

The optional `agents/openai.yaml` supplies Codex display metadata. The instructions themselves do not require Codex, Cursor, or a particular MCP transport. The host must support this skill format; this repository does not install plugins or configure credentials automatically.

Choose an authorized API access path separately. A skill gives the agent guidance, while a client or MCP server lets it execute operations. Installation does not grant permission to send messages or alter a channel.

## Repository layout

- `skills/1msg-integration/SKILL.md` — scope, reference routing, source authority, agent workflow, and critical boundaries.
- `skills/1msg-integration/references/` — getting started, messaging, webhooks, reliability, tools, advanced features, and troubleshooting.
- `skills/1msg-integration/agents/openai.yaml` — optional host metadata.

Future work may add separate `migrate-to-1msg` and `review-1msg-integration` skills, a Cursor adapter under `plugins/cursor`, and maintained executable material under `examples`. Those components are not implemented in this release. Add them only with their own scope, checks, and working content.

## Maintained sources

Use the [1MSG API reference](https://docs.1msg.io/) for operation contracts, [1MSG help](https://help.1msg.io/) for onboarding, and official [SDK](https://github.com/1msg/1msg-sdk), [CLI](https://github.com/1msg/1msg-cli), and [MCP](https://github.com/1msg/1msg-mcp) documentation for client behavior. Meta documentation determines WhatsApp policy and eligibility; channel observations determine the current connection's state.

These instructions are guidance, not a guarantee of service behavior or feature availability. Keep credentials, private comments, production payloads, and internal audit materials outside this repository.

## Distribution

A license has not been assigned. No open-source license or third-party access grant is implied by this package.
