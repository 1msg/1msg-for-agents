# Choose and verify a tool

Use this reference when choosing an access path or diagnosing disagreement between a client and the API. Preserve the user's existing language and tooling unless a concrete limitation requires a change. A skill provides instructions; an MCP connection or another client supplies execution capability.

## Direct HTTP

Use direct HTTP for a minimal integration, an automation platform, or to isolate a client serialization problem. Resolve the operation and request shape from the [1MSG API reference](https://docs.1msg.io/); do not invent an endpoint or copy a Meta Graph API request into the 1MSG API.

Take the server address and channel ID from the same connection. Check whether configuration expects a server address or a complete channel URL to avoid appending the ID twice. Prefer the documented bearer header for direct requests and send the token in one place. A stale token in the body or query can override the header. Redact credentials from all representations before saving diagnostics.

Read status before a permitted send. Interpret the response using [Messaging](messaging.md). HTTP success does not settle delivery or business outcome.

## SDK

Choose a supported client from the [1MSG SDK repository](https://github.com/1msg/1msg-sdk). Confirm the actual language package and installed version before selecting imports, method names, or configuration parameters. Different language clients need not expose identical interfaces.

Configure the same server, channel, and credential used for the verified connection. Keep credential-bearing clients on the server. Pin the version tested in the user's project, and recheck its relevant paths after an update; do not embed a preferred version into this skill.

If an operation is missing, verify the current REST contract and client support. Use a documented direct request only within the same authorization. Do not fabricate a wrapper method or assume that upgrading supplies it. If a client places credentials in query parameters, redact them from access logs and tracing.

## CLI

Consult the [CLI documentation](https://github.com/1msg/1msg-cli) and installed help for command names and options. Inspect the selected profile, server, and channel before running an operation. Defaults can point to production; a profile name does not prove that a channel is a sandbox. Local profiles are not an inventory of all account channels.

Start with read-only state and template inspection. Prefer structured output for comparison with API responses. A successful process exit is not a delivery receipt. Protect profile files because they contain credentials; never include their contents in a report.

## MCP

Use the [MCP server documentation](https://github.com/1msg/1msg-mcp) for supported transports, deployment modes, connection fields, and authentication. Inspect the actual advertised tools and schemas before calling them. Do not assume parity between the hosted server, a locally installed package, and REST.

Verify the connection with a read-only operation. A connected server does not authorize sends or configuration changes, and cannot establish recipient consent. Match every mutating call to the authorized channel, recipients, and intended effect.

If a tool is absent or rejects input, compare its schema, server version, and operation contract. Report a capability mismatch precisely; do not substitute a guessed tool name. A direct REST fallback is appropriate only when its contract, access, and authorization are established.
