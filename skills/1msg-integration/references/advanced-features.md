# Add a specific capability

Use this reference after identifying the required capability. For a new integration, first verify the basic message and reply path. For an existing integration, reuse that evidence where it remains applicable. Inspect current 1MSG support and Meta eligibility for the actual number, region, provider, and permissions before making the feature a dependency.

Read only the section for the requested capability, then the linked maintained documentation. All sends remain subject to [Messaging](messaging.md); all new event handlers remain subject to [Webhooks](webhooks.md) and [Reliability](reliability.md).

## Media

Verify the file's actual type, codec, size, and download accessibility against [Meta media documentation](https://developers.facebook.com/documentation/business-messaging/whatsapp/business-phone-numbers/media#supported-media-types) and the selected [1MSG operation](https://docs.1msg.io/). A browser-authenticated link may be inaccessible to WhatsApp. Do not assume a temporary media URL is permanent storage.

Choose session media or an approved media template according to the recipient's messaging state. Verify that the recipient can open the attachment and that the receiving application can handle the required inbound media. A successful upload alone does not prove delivery.

## Buttons and lists

Select the supported interaction from the [1MSG API reference](https://docs.1msg.io/). Distinguish session interactions from approved template buttons. Map stable response identifiers to business actions rather than depending only on displayed labels.

Verify the response event and resulting action. Delivery of a menu does not establish a selection. Suppress repeat actions and provide a route to free text or a human when the interaction cannot complete the task.

## WhatsApp Flows

Use the [1MSG Flows guide](https://help.1msg.io/docs/api-1msg/whatsapp-flow) and the current API contract. Distinguish an actual WhatsApp form from an ordinary chatbot workflow. Check form creation, validation, publication, sending, and response handling separately; support for one stage does not prove support for all stages.

Use a simple form unless live data exchange is required. When it is required, implement the documented protected exchange and validate returned data. Associate completed submissions with the correct request. Opening or delivering the Flow is not a completed submission. Check the approved template route when sending outside the permitted session.

## Catalog and commerce

Verify catalog linkage and the required capability in the [1MSG catalog reference](https://docs.1msg.io/#operation/getCommerce). Product messages refer to existing catalog items. Validate a submitted cart against the merchant's current inventory and order rules before creating an order.

Keep cart submission, order creation, invoice delivery, and confirmed payment separate. Only the payment system's authoritative result can confirm payment. Region-specific address, invoice, and payment features require separate eligibility checks. If unsupported, propose a permitted link to the merchant's own checkout without claiming native payment support.

## Calling

Check [Meta Calling](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling), [call permissions](https://developers.facebook.com/documentation/business-messaging/whatsapp/calling/user-call-permissions), and the actual 1MSG contract. Establish who handles audio and which WebRTC or supported SIP component supplies it; API call control alone is not an audio client.

Verify number and regional eligibility, connection compatibility, and valid recipient permission for an outbound call. Messaging consent is not call permission. Do not assume call recording or history is included. Verify signaling, audio, termination, and failure handling with an authorized participant.

## Groups

Check [Meta Groups](https://developers.facebook.com/documentation/business-messaging/whatsapp/groups) and the supported 1MSG operations. An existing phone-app group does not automatically become an API group. Verify supported message types and group-specific restrictions instead of inheriting the whole direct-message feature set.

Distinguish the group destination from each participant's identity. Verify invitation, membership changes, messages, and leaving. Ensure the intended audience and consent match a shared conversation before taking actions that expose messages to other participants.

## Coexistence

Follow [1MSG Coexistence onboarding](https://help.1msg.io/docs/onboarding/waba-coexistence). Verify current eligibility and connection maintenance requirements instead of hardcoding a waiting period or promising admission. Coexistence is a specific route for a suitable WhatsApp Business App number, not a universal reversal of API onboarding.

Agree how automation and staff share the conversation. Treat phone-app outgoing echoes as company messages, not new customer messages; they must not trigger bot loops or refresh the customer reply window. Verify customer input, staff output, and API output separately. Establish historical import availability separately from delivery of new events.
