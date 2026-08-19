---
name: pabbly-verify-and-send-email-campaign
description: Verify an email address with Pabbly Email Verification, then create the subscriber, put them on a list, and send a Pabbly Email Marketing campaign to that list.
apis:
  - pabbly:pabbly-email-verification
  - pabbly:pabbly-email-marketing
base_urls:
  - https://verify.pabbly.com/api/v1
  - https://emails.pabbly.com/api/v2
auth: >-
  Email Verification uses HTTP Basic (API Key : Secret Key). Email Marketing uses
  Bearer token. They are separate credentials from separate dashboards.
docs: https://apidocs.pabbly.com/email-marketing/reference
generated: '2026-08-13'
method: generated
source: https://apidocs.pabbly.com/pabbly/email-marketing/llms.txt
operations:
  - POST /email-lists/verify-single
  - POST /subscribers
  - GET /subscribers/{email}
  - GET /custom-fields
  - GET /lists
  - POST /lists
  - POST /lists/add-subscriber
  - POST /lists/remove-subscriber
  - POST /lists/move-subscriber
  - POST /campaigns
  - GET /campaigns
  - POST /campaigns/send-to-list
  - POST /campaigns/send-to-individual
  - GET /delivery-servers
  - GET /subscribers/stats
---

# Pabbly — verify, subscribe, send

Two APIs, two credentials, two hosts. Do not mix them up: the verification call
is HTTP Basic on `verify.pabbly.com`, everything else is a Bearer token on
`emails.pabbly.com` — and Email Marketing is on **v2** while every other Pabbly
API is on v1.

## 1. Verify the address first

`POST https://verify.pabbly.com/api/v1/email-lists/verify-single`
Body: `{"email": "<address>"}` (required).

The response `data` carries `result` plus the risk flags that matter:

| Field | Meaning |
|---|---|
| `result` | e.g. `undeliverable` — the verdict |
| `message` | why, e.g. "Unreachable mail server. Emails sent to this address will bounce." |
| `accept_all` | catch-all domain (verdict is unreliable) |
| `disposable` | throwaway address |
| `spamtrap` | do not send, ever |
| `role` | role account (info@, support@) |
| `free_email` | consumer mailbox |

Gate on this. A `spamtrap` or `undeliverable` result should stop the flow, not
just get logged — Pabbly's own positioning for this product is sender-reputation
protection.

Verification data is deleted after 15 days on Pabbly's side, so if you need the
verdict later, persist it yourself.

## 2. Create or update the subscriber

`POST https://emails.pabbly.com/api/v2/subscribers` — this is an **upsert**: an
existing email is updated rather than duplicated, which is the closest thing to
idempotency anywhere on the Pabbly surface. Use it instead of a read-then-create.

- `email` (required)
- `firstName`, `lastName`, `mobile` (with country code), `country`, `city`
- `leadScore` — 0–100
- `status` — `subscribed` | `unsubscribed` | `bounced` | `complaint`
  (default `subscribed`)
- `tags` — array of tag names, **which must already exist in Settings**
- `customFields` — check what exists first with `GET /custom-fields`

Read back with `GET /subscribers/{email}` or `GET /subscribers/{id}`.

## 3. Put them on a list

- List the lists: `GET /lists`, or `GET /lists-and-segments`
- Create one: `POST /lists`
- Attach: `POST /lists/add-subscriber` with `listIds` (required array, at least
  one id) and **either** `email` **or** `subscriberId`. Already-member lists are
  skipped, so this call is safe to repeat.
- Also available: `POST /lists/remove-subscriber`, `POST /lists/move-subscriber`

## 4. Send

- Create the campaign: `POST /campaigns`; confirm with `GET /campaigns`.
- Send to lists: `POST /campaigns/send-to-list` with `campaignId`,
  `includedList` (the lists/segments to send to), optional `excludedList`,
  `deliveryServerId`, `senderEmail`.
- One-off: `POST /campaigns/send-to-individual`.

Preconditions Pabbly states for `send-to-list`: the campaign must exist, belong
to your business, and have status **live**; the lists must exist. Only
subscribers with status `subscribed`, not deleted and not suppressed are sent to,
and duplicates across lists are deduplicated.

**This call is asynchronous.** It queues in batches and returns immediately, so a
200 means "accepted", not "delivered". Do not treat the response as a send
receipt — poll `GET /subscribers/stats` for outcome.

Sending needs a delivery server (your own SMTP — SES, SendGrid, Postmark,
Mailgun). Confirm one is attached with `GET /delivery-servers` before the first
send.

## Errors and limits

Standard Pabbly status classes: 400 malformed, 401 bad credentials, 404 missing,
429 too many requests, 5xx retry with backoff. Failure bodies are prose
(`{"success": false, "error": "…"}`) with no error code.

No rate limit is published for either API and no `X-RateLimit-*` headers are
returned, so on a large list treat 429 as a signal to slow down globally rather
than to retry the single call. See `rate-limits/pabbly-rate-limits.yml`.
