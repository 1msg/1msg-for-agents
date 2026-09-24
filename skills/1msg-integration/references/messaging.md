# Send and verify messages

Load this reference when choosing a message format, using a template, interpreting a send result, or resolving an uncertain send. Obtain request fields and supported formats from the [1MSG API reference](https://docs.1msg.io/), not remembered SDK signatures or invented endpoints.

## Establish whether the send is permitted

1. Confirm the intended channel, recipient, content, and existing authorization. Use one destination field supported by the operation. Preserve an observed conversation identifier rather than guessing a phone number from it.
2. Determine the customer's last genuine inbound message and its timestamp. The customer-service window runs for 24 hours from that message and restarts on another customer message. Business messages, templates, read receipts, and outgoing Business App echoes do not open or extend it.
3. Use supported free-form messages only with evidence that the window is open. Otherwise choose an approved template or obtain the missing evidence before a free-form send. An empty or disabled history response does not establish the customer's activity; do not treat incomplete history as authoritative proof.
4. Check recipient consent and opt-out handling independently. Consult the [WhatsApp Business Messaging Policy](https://business.whatsapp.com/policy) for the current rule applicable to the request. An open window is not permission for unrelated messages.

A free-entry-point benefit does not extend the customer-service window. Do not infer price or eligibility from the chosen format. Retrieve current official policy if that distinction affects the task; do not embed tariff tables in agent instructions.

## Select the actual message

For a template, inspect the channel's current templates and confirm approval, exact name, language, namespace, and required components. Follow [Send Template](https://docs.1msg.io/#operation/sendTemplate). If required metadata is absent, resolve it instead of inventing a namespace. Match media, buttons, and variables to the approved structure. A created template is not necessarily approved.

Choose a template category from the business purpose under current Meta rules. Do not label promotional content as transactional merely to make it pass. For template changes, inspect the operation's scope: deleting by name may affect more than one language.

Auto-template behavior depends on the channel configuration. It does not remove approval, consent, or format requirements. Prefer an explicit approved template outside the window rather than relying on an unverified fallback. Sending a starter template does not authorize a subsequent free-form attachment without a customer reply.

## Interpret the result before another action

- Inspect the response body even with HTTP 200. Preserve the operation outcome and relevant IDs; do not invent a uniform error wrapper for all operations.
- A positive `sent` result is not proof of delivery. Verify delivery events separately; absence of a read event is not proof of failure.
- A queued result may accompany a negative `sent` field. Preserve `jobId` as a queue job identifier, not as a message ID. Do not resend simply because processing is pending.
- Use [Check ACKs](https://docs.1msg.io/#operation/ackHookInfo) only with the required message ID. Do not invent polling or a conversion from `jobId` to message ID.

After a timeout or ambiguous failure, reconcile available events, the recipient's observed result, and the delivery log before retrying. If only a job ID remains, retain channel, recipient, time, and safe outcome details for support. Report the result as unknown until resolved. Use [reliability](reliability.md) when implementing retry behavior.
