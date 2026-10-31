# Pabbly

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Pabbly is an Indian SaaS company selling a suite of marketing and sales software as one-time lifetime deals as well as monthly plans: Pabbly Connect (workflow automation), Pabbly Subscription Billing (recurring billing and invoicing), Pabbly Email Marketing, Pabbly Chatflow (WhatsApp Business messaging), Pabbly Form Builder, Pabbly Hook (an inbound webhook gateway) and Pabbly Email Verification. Its developer platform at apidocs.pabbly.com documents five REST APIs and 173 endpoints, all keyed on HTTP Basic or Bearer credentials generated from the account dashboard, plus a sixth Connect Platform API distributed only through a first-party npm SDK. Pabbly Subscription Billing emits 23 webhook event types, and Pabbly Connect ships a beta hosted MCP server that exposes a customer's own workflow actions as agent tools.

**Areas:** Automation · Email Marketing · Subscription Billing · Billing · Payments · Webhook · Messaging · WhatsApp · Email Verification · Forms · No-Code · Software-as-a-Service

## Pabbly on APIs.io

Everything profiled for Pabbly is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/pabbly/](https://apis.io/providers/pabbly/)** — or go straight to one of the APIs or properties below.

### APIs (7)

- [Pabbly Subscription Billing API](https://apis.io/apis/pabbly/pabbly-subscriptions/) — REST API for Pabbly Subscription Billing — 95 documented operations across customers, subscriptions, products, plans, multiplans, coupons, invoices, payment…
- [Pabbly Hook API](https://apis.io/apis/pabbly/pabbly-hook/) — REST API for Pabbly Hook, an inbound webhook gateway that receives, logs, filters, transforms and forwards webhook requests. 21 documented operations across…
- [Pabbly Chatflow API](https://apis.io/apis/pabbly/pabbly-chatflow/) — REST API for Pabbly Chatflow, a WhatsApp Business messaging platform. 33 documented operations covering 18 message shapes on a single POST /messages…
- [Pabbly Email Marketing API](https://apis.io/apis/pabbly/pabbly-email-marketing/) — REST API for Pabbly Email Marketing — 23 documented operations across subscribers (upsert semantics on create), subscriber lists and segments, campaigns…
- [Pabbly Email Verification API](https://apis.io/apis/pabbly/pabbly-email-verification/) — REST API for Pabbly Email Verification. A single operation, POST /email-lists/verify-single, returning a deliverability verdict plus accept_all, disposable,…
- [Pabbly Connect Platform API](https://apis.io/apis/pabbly/pabbly-connect-platform/) — Backend-to-backend API that lets a product use Pabbly Connect as its connection manager — search a catalog of 1,000+ apps, mint hosted OAuth or credential…
- [Pabbly Connect MCP Server](https://apis.io/apis/pabbly/pabbly-connect-mcp/) — Beta hosted MCP server that exposes a Pabbly Connect user's own workflow action steps as agent tools. Tools are authored inside Pabbly Connect ("Add to MCP…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Pabbly Subscription Billing Webhooks](https://apis.io/asyncapis/pabbly/pabbly-subscription-billing-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Pabbly Connect MCP Server](https://apis.io/servers/pabbly/pabbly-connect-mcp-server/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Pabbly Authentication](https://apis.io/security/pabbly/pabbly-authentication/)
- [Pabbly Domain Security](https://apis.io/security/pabbly/pabbly-domain-security/)
- [Pabbly Trust Center](https://apis.io/security/pabbly/pabbly-trust-center/)
- [Pabbly Vulnerability Disclosure](https://apis.io/security/pabbly/pabbly-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Pabbly Plans Pricing](https://apis.io/plans/pabbly/pabbly-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Pabbly Rate Limits](https://apis.io/rate-limits/pabbly/pabbly-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Pabbly Finops](https://apis.io/finops/pabbly/pabbly-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.pabbly.com/)
- [Documentation](https://apidocs.pabbly.com/)
- [Getting started](https://apidocs.pabbly.com/subscription-billing/reference/guides/overview)
- [API reference](https://apidocs.pabbly.com/subscription-billing/reference)
- [Pricing](https://buy.pabbly.com/pabbly-plus/)
- [Sign up](https://accounts.pabbly.com/signup)
- [Support](https://www.pabbly.com/contact-us/)
- [Blog](https://www.pabbly.com/blog/)
- [Security](https://www.pabbly.com/security-vulnerability-disclosure/)
- [Compliance](https://www.pabbly.com/security/)
- [Terms of service](https://www.pabbly.com/terms-conditions/)
- [Privacy policy](https://www.pabbly.com/privacy-policy/)
- [GitHub](https://github.com/Pabbly)

## API Evangelist rating

**Kin Score 53.2** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 68.3 |
| Contract quality | 39.0 |
| Contract governance | 4.5 |
| Operational transparency | 26.3 |
| Developer ergonomics | 68.5 |
| Access clarity | 85.5 |

**Agent readiness 29.5** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | na |

**Regulatory alignment 32.1** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/pabbly/](https://apis.io/providers/pabbly/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Pabbly: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/pabbly/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
