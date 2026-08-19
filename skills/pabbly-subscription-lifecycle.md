---
name: pabbly-subscription-lifecycle
description: Create, inspect, change and cancel a subscription in Pabbly Subscription Billing, including the customer record it hangs off.
api: pabbly:pabbly-subscriptions
base_url: https://payments.pabbly.com/api/v1
auth: HTTP Basic — API Key as username, Secret Key as password
docs: https://apidocs.pabbly.com/subscription-billing/reference
generated: '2026-08-13'
method: generated
source: https://apidocs.pabbly.com/pabbly/subscription-billing/llms.txt
operations:
  - POST /customer
  - GET /customer/{customer_id}
  - GET /customer/
  - POST /subscription
  - POST /subscription/{customer_id}
  - GET /subscription/{subscription_id}
  - GET /subscriptions/{customer_id}
  - PUT /subscription/{subscription_id}/upgrade-downgrade
  - PUT /subscription/{subscription_id}/pause
  - PUT /subscription/{subscription_id}/resume
  - PUT /subscription/change-billing/{subscription_id}
  - POST /subscription/{subscription_id}/cancel
  - GET /scheduledchanges/{subscription_id}
---

# Pabbly Subscription Billing — subscription lifecycle

Every path below is relative to `https://payments.pabbly.com/api/v1`. Every request
carries HTTP Basic auth: API Key as the username, Secret Key as the password.
Send `Content-Type: application/json` on any request that has a body.

> **No idempotency.** Pabbly publishes no idempotency key on any operation. A
> retried `POST /subscription` can create a second subscription. Before retrying
> any create, re-read state (`GET /subscriptions/{customer_id}`) and only retry
> if the object is genuinely absent.

## 1. Find or create the customer

- Look up by id: `GET /customer/{customer_id}`
- Look up by email: `GET /customer/?email=<email>`
- Create: `POST /customer` — required body fields are `first_name`, `last_name`,
  `email_id`. Optional: `company_name`, `website`, `phone`, `billing_address`,
  `shipping_address`, `is_affiliate`.

A success looks like `{"status":"success","message":"Customer Created","data":{...}}`.
Note the envelope: the guides document `{"success": true, "data": …}` but every
worked example returns `status`/`message`/`data`. Parse defensively for both.

## 2. Start the subscription

Two entry points, and picking the wrong one duplicates the customer:

- **New customer + subscription in one call:** `POST /subscription`. Body carries
  the customer fields plus `plan_id`, `plan_amount`, `quantity`, `gateway_type`,
  `gateway_id`, optional `coupon_code`, `addons`, `redirect_to`. Returns a new
  customer id **and** a subscription id.
- **Existing customer:** `POST /subscription/{customer_id}`. Use this whenever
  step 1 found a customer — never `POST /subscription`.

`gateway_type` is one of `paypal`, `stripe`, `test`, `custom`, `connect`,
`offline`, `free`. Razorpay and Authorize.net use `custom`; Paddle, Adyen and
Instamojo use `connect`.

If you would rather not handle card data, do not send `card_number`/`month`/
`year`/`cvv` — mint a hosted page instead (`GET /checkoutpage/{product_id}` or
`GET /add_card_url/{customer_id}`) and let the customer enter it on Pabbly.

## 3. Read state

- One subscription: `GET /subscription/{subscription_id}`
- All for a customer: `GET /subscriptions/{customer_id}`
- Pending scheduled changes: `GET /scheduledchanges/{subscription_id}`

There is no documented pagination contract on the list operations. Do not assume
a `page` or `limit` parameter works.

## 4. Change it

- Plan change: `PUT /subscription/{subscription_id}/upgrade-downgrade`
- Pause / resume: `PUT /subscription/{subscription_id}/pause`,
  `PUT /subscription/{subscription_id}/resume`
- Move the billing date: `PUT /subscription/change-billing/{subscription_id}`
- Adjust charges: `POST /subscription/{subscription_id}/update_charges`
- Credit: `POST /subscription/credit/{subscription_id}/create` and
  `.../deduct`

## 5. Cancel

`POST /subscription/{subscription_id}/cancel` with body:

- `cancel_at_end` (required) — `true` cancels at the end of the term,
  `false` cancels immediately.
- `cancel_reason` (optional) — defaults to `"Cancelled via API."`

Confirm from the response message, then verify with
`GET /subscription/{subscription_id}`.

## Error handling

| Status | Do this |
|---|---|
| 400 | Fix the payload. Do not retry unchanged. |
| 401 | Credentials are wrong or revoked. Regenerate at Settings → API Settings. Do not retry. |
| 404 | The id does not exist. Terminal. |
| 429 | Back off. No limit, window or reset header is published, so use exponential backoff with jitter. |
| 5xx | Retry with backoff — but re-read state first on any create. |

Failure bodies are `{"success": false, "error": "<prose>"}`. There is no error
code to branch on; only the HTTP status is reliable.

## Staying in sync

Pabbly emits 23 webhook events for this product — `Subscription Create`,
`Subscription Activate`, `Subscription Renew`, `Subscription Cancel`,
`Subscription Trial Expired`, `Payment Failure`, `Dunning Invoice` and more.
See `asyncapi/pabbly-subscription-billing-webhooks.yml`. Payloads carry **no
signature**, so verify anything consequential by reading it back from the API
before acting on it.
