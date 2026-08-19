---
name: pabbly-whatsapp-broadcast
description: Send a WhatsApp template message or run a broadcast to contact lists through the Pabbly Chatflow API.
api: pabbly:pabbly-chatflow
base_url: https://chatflow.pabbly.com/api/v1
auth: Bearer token in the Authorization header
docs: https://apidocs.pabbly.com/chatflow/reference
generated: '2026-08-13'
method: generated
source: https://apidocs.pabbly.com/pabbly/chatflow/llms.txt
operations:
  - GET /templates
  - GET /templates/{identifier}
  - POST /contacts
  - GET /contacts/
  - GET /contacts/{id}
  - PUT /contacts/{id}
  - GET /contacts/lists
  - GET /settings/tags
  - POST /messages
  - POST /broadcasts
  - POST /broadcasts/send
  - GET /media
  - GET /catalogs
  - GET /catalogs/{catalogId}/products
---

# Pabbly Chatflow — WhatsApp templates and broadcasts

Base `https://chatflow.pabbly.com/api/v1`. Auth is
`Authorization: Bearer <API_KEY>`, taken from Chatflow → Settings → API & Webhooks.
Note this differs from Subscription Billing, Hook and Email Verification, which
use HTTP Basic.

## 0. Know what you are billed for

One credit = one outgoing WhatsApp message. **Meta charges separately, per
conversation, and Pabbly adds 0% markup on top** — so a broadcast has two cost
meters, only one of which Pabbly meters. Check the plan quota before a large
send (`plans/pabbly-plans-pricing.yml`).

## 1. Resolve the template

WhatsApp requires an approved template for anything outside a 24-hour customer
service window. Templates are created in Chatflow or WhatsApp Manager, not
through this API.

- `GET /templates` — query `all` (any truthy value for everything), `limit`
  (default 50), `page` (default 1). This is the **only** Pabbly endpoint in the
  whole catalog with a documented page/limit pagination contract.
- `GET /templates/{identifier}` — by name or id.

Read `components` on the returned template to learn how many `{{1}}`-style
placeholders the header and body take. Getting the parameter count wrong is the
most common send failure.

## 2. Make sure the contacts exist

- `POST /contacts` — `mobile` (required, with dialing code) plus `name`,
  `optin`, `incomingBlocked`, `tags`, `attributes`.
  - `onDuplicate` controls collision behavior: `skip` (default) or overwrite.
    That default is what makes this call safe to repeat — use it instead of
    read-then-create, because Pabbly has no idempotency keys.
  - `tags` and `attributes` (custom fields) **must already exist in Settings**;
    confirm with `GET /settings/tags`.
- `GET /contacts/`, `GET /contacts/{id}`, `PUT /contacts/{id}` to read and update.
- `GET /contacts/lists` for the contact lists you can target.

## 3a. Send one message

`POST /messages`. Required: `to` (recipient number with dialing code, e.g.
`15551486942`), `type`, and for template sends `templateName`.

- Template send: `type: "template"`, plus `headerParams` / `bodyParams` arrays
  positionally matching `{{1}}`, `{{2}}`, …
- The same `POST /messages` path also carries free-form sends (text, image,
  audio, video, document, location/address requests) and rich types (list, CTA
  URL, single/multi product, catalog) — 18 documented shapes on one endpoint,
  discriminated entirely by the body. There is no separate path per message
  type, so the body is the contract.
- Optional `contact` object upserts the contact matched to the recipient number,
  saving a separate `POST /contacts`.

Media: `GET /media` retrieves a media URL. Commerce: `GET /catalogs` and
`GET /catalogs/{catalogId}/products` back the product and catalog message types.

## 3b. Or run a broadcast

`POST /broadcasts` with:

- `name` (required)
- `broadcastType` — must be `"broadcast"`
- `messageType` — `"template"` or `"regular"`
- `status` — `"scheduled"` or `"instant"`
- `contactList` (required for `broadcastType: "broadcast"`) — array of contact
  list **names**; include `"unassigned"` to reach contacts on no list
- `templateName` when `messageType` is `"template"`, with `headerParams` /
  `bodyParams`

For the API-campaign flow, `POST /broadcasts` registers the campaign and
`POST /broadcasts/send` triggers it.

## Errors

400 malformed, 401 bad/missing key, 404 not found, 429 too many requests, 5xx
retry with backoff. Bodies are `{"success": false, "error": "<prose>"}` — no
error code, so branch on the status only.

No rate limit or throttle ceiling is published for Chatflow, and no
`X-RateLimit-*` headers are returned. On a broadcast, pace yourself rather than
discovering the ceiling with a 429.
