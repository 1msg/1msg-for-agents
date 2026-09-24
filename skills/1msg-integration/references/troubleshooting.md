# Locate the failed stage

Use this reference for a specific failure. Start from one request or message and trace connection, API response, WhatsApp delivery, webhook delivery, durable receipt, and application processing. Inspect evidence before changing configuration. Do not replay an uncertain send as a diagnostic shortcut.

| Observation | Investigation and next decision |
| --- | --- |
| API access fails | Match server, channel ID, and credential; inspect HTTP status and response body. Remove conflicting credential locations without exposing their values. Verify the operation contract before changing the request. |
| Channel is connected but no message arrives | Check recipient, message eligibility, response body, queue state, and delivery evidence. Inspect available account restrictions, payment status, and channel sending limits when relevant to the returned error; do not infer them from connectivity or invent prices. Preserve message ID and job ID separately. For an uncertain outcome, follow [Messaging](messaging.md) before another send. |
| Template is rejected or unusable | Inspect approval state, exact name and language, components, variables, category, and the channel's permissions. Use the returned reason and current documented rules; do not guess a replacement category or approval outcome. |
| The dashboard has a message but the application does not | Inspect the complete recipient list, the target's delivery state, reachable HTTPS handler, selected payload format, notification settings, and a new authorized inbound event. Dashboard visibility does not prove webhook receipt. |
| Another integration stopped after webhook setup | Compare the before and after recipient lists. Restore the intended complete list only within authorization, then verify every affected recipient. See [Webhooks](webhooks.md). |
| A webhook is disabled | Fix the receiver first, inspect per-URL state, and follow the documented recovery path in [Reliability](reliability.md). Restoring delivery does not prove that missed events were replayed. |
| History is empty | Verify whether history is saved, the filters, pagination, time units, and the connection's contract. Disabled or incomplete history does not prove that no messages existed. |
| Duplicate actions or bot self-replies | Inspect event type, message direction, channel-scoped deduplication keys, status handling, multiple consumers, and the business operation's persisted result. |
| HTTP works but a client fails | Compare the installed client or advertised tool schema, endpoint configuration, serialization, and the actual REST request. Use [Tools](tools.md); do not invent missing methods. |

## Preserve usable evidence

Record the channel ID, time with timezone, operation, HTTP status, redacted request and response, message ID or queue job ID, expected outcome, actual outcome, and the checks performed. Record the failed stage explicitly. Include a sanitized event or reproducible artifact only when it is needed for diagnosis.

Remove tokens from headers, bodies, query parameters, traces, and profile files. Redact credential-bearing webhook URLs and unnecessary customer data. Do not paste private conversation history into an issue or support request by default.

## Stop when evidence is insufficient

Consult the [API reference](https://docs.1msg.io/) and [1MSG help](https://help.1msg.io/) for the specific behavior. If access, deployment, contract, or operation outcome remains uncertain, state exactly what is unknown and what evidence would resolve it. Prepare a support handoff with the sanitized facts. Send it only if the user has authorized the communication; preparation alone is not permission to contact support.
