# Receive and interpret events

Load this reference when adding a receiver, changing webhook destinations or format, or connecting inbound messages to application behavior. Read [Set webhook URL and callback payload samples](https://docs.1msg.io/#tag/Webhooks/operation/setWebhook) for the actual request and event contracts.

WhatsApp Business Platform sends message and delivery-status notifications to 1MSG, directly or through the channel's provider. 1MSG then delivers the relevant events to the configured application webhook. These are separate delivery stages: an application webhook failure does not establish that the WhatsApp message failed.

## Prepare durable acceptance

Use a reachable HTTPS endpoint or the automation platform's supported incoming webhook. A development tunnel can expose a local receiver for a controlled test; it is not a production availability guarantee.

Validate the request using a mechanism supported by the channel's delivery contract. Do not assume that incoming requests contain the channel's API token or a Meta signature. An `instanceId` field is not authentication, and an unguessable URL is not a cryptographic signature. Establish the supported origin check before promising authenticated delivery.

Accept and durably store the entire payload before returning a successful HTTP response. Await the database commit or durable queue acceptance; starting a background write is insufficient. If storage fails, do not acknowledge success. Run slow CRM, AI, and business processing separately. Check the applicable delivery timeout and leave a margin rather than making receipt depend on downstream services.

Test storage failure and a normal POST before registering the endpoint. Preserve unfamiliar event types for inspection instead of acknowledging and discarding them.

## Preserve existing destinations and settings

1. Read the current webhook list and relevant settings. Identify destinations used by the 1MSG cabinet and other integrations.
2. Construct the complete intended list. `webhookUrl` replaces that list; it does not append one address. Check the current supported limit and request shape. Preserve existing destinations unless their removal is authorized.
3. Re-read immediately before applying the replacement so a concurrent addition is not lost. Apply the authorized change, read it back, and compare the full list. For general settings, submit only fields documented as writable and needed for the change; do not echo an entire GET response into POST. Verify fresh-event receipt and processing at the new destination and all preserved consumers.
4. Check `ackNotificationsOn` when delivery statuses are required. Inspect `rawHooks` and the receiver's expected format before changing either.

With `rawHooks`, verify the actual payload delivered by the target channel. Do not assume that normalized and raw formats always arrive together. Observing one format alone does not prove that the other is absent. If documentation and observed behavior disagree, record the mismatch and resolve the receiver contract before changing processing. Do not silently switch a working channel's format.

## Route every event correctly

In the normalized format, inspect all elements of `messages` and `ack`; one HTTP request can contain multiple events. Confirm the selected format rather than applying these array names to an arbitrary raw provider payload.

Preserve channel ID, message ID, conversation identifier, direction, event type, timestamp, and required content. Treat `chatId` and BSUID as opaque strings within their channel or business context. Do not infer support for arbitrary username addressing or merge unrelated contacts by guesswork. Verify timestamp units before calculating the response window.

Check `fromMe` and event type before invoking a bot. A company message or employee echo must not trigger an automatic reply as a new customer request. Update customer activity only from a genuine inbound customer event.

HTTP acknowledgement of receipt, a WhatsApp delivery status, and marking a customer message read are separate operations. Verify a fresh inbound event after configuration, then use [reliability](reliability.md) for deduplication, processing failures, and disabled destinations.
