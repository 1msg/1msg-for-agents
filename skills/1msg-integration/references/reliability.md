# Handle retries, outages, and uncertain outcomes

Load this reference for unattended processing, webhook recovery, send retries, or a production readiness check. Keep transport receipt, application processing, and the outcome of external actions as separate states. For receiver setup and storage-before-acknowledgement, use [webhooks](webhooks.md).

## Make repeated delivery safe

Persist every accepted event before acknowledging receipt. Process saved events with durable state so a restart can resume unfinished work. Do not mark an event fully processed merely because it was received or a worker started.

Scope incoming-message deduplication by channel and message ID, with event kind where different kinds share storage. For delivery notifications, include the status: sent, delivered, and read for one message are distinct changes. Handle late events without regressing a confirmed delivery state. Do not suppress all subsequent statuses after seeing the first message ID.

If raw and normalized callbacks reach the same business processing path, verify the documented mapping to a common event identity before deduplicating across formats. Retain original payloads; do not guess matching fields from a single sample or execute the same business action once per representation.

Store a business action's intent with the processing result before dispatching it externally. A durable outbox helps restart recovery but does not make an external send exactly once. If an external service acted and its response was lost, reconcile before repeating that action. Keep failed or unfamiliar events available for investigation and controlled replay.

## Bound retries according to the operation

Retry temporary read failures with bounded backoff. Correct authentication or validation errors instead of repeating them. When rate-limited, reduce traffic and follow a supplied retry delay; do not assume every API surface provides one.

A timeout or server error during a send can leave an unknown result. Follow [messaging](messaging.md) to reconcile it. Never turn uncertainty into a new send by applying a generic HTTP retry policy. Preserve queue job IDs separately from message IDs and do not invent a job-result endpoint.

Automate a repeat send only when the applicable contract and evidence establish a retryable failure without prior acceptance, or a verified idempotency mechanism covers that exact operation. Do not assume 1MSG provides such a mechanism. Elapsed time alone does not convert an unknown outcome into failure. Keep any permitted retries bounded and within the original sending authorization.

Inspect `guaranteedHooks`, actual runtime behavior, and the applicable retention and retry contract. Do not promise unlimited retries, strict ordering, or complete replay from the setting's name or a general “until acknowledged” description. If documentation and the target behavior conflict, disclose that limitation before depending on it. Maintain your own durable processing and alerting.

## Recover the affected destination

Read per-URL `webhookStatuses` using [Get webhook URL](https://docs.1msg.io/#tag/Webhooks/operation/getWebhook). One disabled destination does not mean that all destinations or the channel are down. Monitor endpoint availability, acknowledgement latency, storage failures, processing backlog, and unresolved external outcomes.

After correcting the receiver, determine whether the affected destination needs reactivation. When needed and authorized, re-read and submit the complete intended destination list through [Set webhook URL](https://docs.1msg.io/#tag/Webhooks/operation/setWebhook), then read back status and verify a fresh event. Do not rely on submitting an unchanged webhook setting through the general settings operation to reset a disabled destination. Preserve other integrations throughout recovery.

Use available history to investigate gaps, accounting for pagination and timestamp units. History may be disabled or incomplete and can differ from the cabinet's view. Neither an empty result nor a successful re-enable proves that no events were missed. Reconcile against your own store and state explicitly which interval remains unverified.

## Demonstrate recovery before unattended use

Check repeated receipt, a restart after persistence, failed persistence, and an external action whose response is lost. Verify both that accepted work survives and that replay does not duplicate the business action. Bound application retries, retain exhausted work for review, and make disabled destinations visible. Ensure a stop mechanism can pause outgoing actions while retaining incoming events. Preserve deduplication records for the relevant replay period and verify backup restoration.

Restrict access to message data and diagnostics, define retention periods, and keep secrets out of logs. For a production transition, verify the actual production connection, permissions, approved templates, consent and opt-out handling, and complete webhook configuration. Recheck one authorized exchange before increasing traffic; do not assume test settings or eligibility transfer to production.
