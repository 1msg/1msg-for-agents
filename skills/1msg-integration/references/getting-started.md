# Establish a working integration

Load this reference for a first integration or a switch between test and production channels. Use the existing project's stack and the user's chosen tool. A request to connect an existing channel does not authorize migrating its number or replacing its other integrations.

## Inspect before asking

1. Identify the application language, server-side entry point, installed 1MSG client, and existing configuration conventions. Determine whether a channel and webhook receiver already exist. Inspect configuration names and wiring without exposing credential values.
2. Establish the channel's API server, 1MSG channel ID, test or production environment, and intended recipient. These IDs are not interchangeable with a telephone number, WABA ID, or Meta Phone Number ID. Do not infer production from a client's default URL.
3. Reuse information and authorization already provided. Ask only for missing facts that affect the next action. If the recipient or permission to send remains unclear, continue read-only setup while obtaining that information.

If the number is not connected, use the relevant [1MSG onboarding guidance](https://help.1msg.io/). Ownership of a WABA does not establish whether Direct Meta onboarding, a provider transfer, or Coexistence is appropriate. Do not perform an unrequested migration.

## Connect and verify access

Obtain credentials through the project's supported secret mechanism. Keep channel credentials on the server and out of generated examples, repository files, browser code, and diagnostic output. Use the authentication mechanism documented for the selected client; avoid conflicting copies of a token in different request locations.

Check how that client constructs its URL. For a server-base configuration, the base contains the API server without the channel ID; supply the ID separately. If the supplied address already contains the channel ID, do not append it again. Read the client's documentation before mapping environment variables or constructor parameters.

Read channel status through the [current API contract](https://docs.1msg.io/). Inspect the response body as well as HTTP status. A connected channel establishes connectivity, not recipient eligibility, availability of every feature, or working inbound delivery. If a manual exchange also fails, resolve channel access before debugging application code.

## Complete one exchange

1. Follow [messaging](messaging.md) to establish the permitted message format and send once to the authorized test recipient. Use current channel-specific test conditions; do not assume a test channel has production capabilities.
2. Record whether the request was accepted, queued, rejected, or remains uncertain. Confirm delivery separately; an API success response alone does not complete the test.
3. Follow [webhooks](webhooks.md) to connect a receiver while preserving existing destinations. Obtain a fresh inbound reply after configuration. Confirm that the application durably saved it and associated it with the correct channel and conversation.
4. Before unattended operation, apply the relevant failure checks in [reliability](reliability.md).

Report the completed exchange and any unverified stage precisely. For a production transition, check the new server, channel ID, credentials, templates, and webhook configuration independently; repeat the exchange with authorized recipients. Do not assume test configuration transfers automatically.
