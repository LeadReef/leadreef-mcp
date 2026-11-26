# LeadReef MCP tool reference


Canonical page: https://leadreef.ai/mcp/docs
Last updated: 2026-11-06 · Server v1.0.0 · MCP 2025-11-25
Setup guides: https://leadreef.ai/mcp#setup

## Connection

- **Server URL:** https://api.leadreef.ai/mcp
- **Transport:** Streamable HTTP. Remote server, nothing to install. Stateless: every request is a POST and gets a JSON response; there are no sessions and no server-sent event stream.
- **Protocol:** MCP 2025-11-25. Clients on 2025-06-18, 2025-03-26 and 2024-11-05 are also accepted.
- **Capabilities:** Tools only. No resources, prompts or sampling.
- **Cannot:** Send messages, delete leads, change billing or publish sites.

## Authorization

OAuth 2.1 with PKCE (S256). The user signs in on leadreef.ai and approves the client there; the client never sees the password. Unauthenticated requests get 401 with a WWW-Authenticate header pointing to the resource metadata.

- **Resource metadata:** https://api.leadreef.ai/.well-known/oauth-protected-resource/mcp
- **Server metadata:** https://api.leadreef.ai/.well-known/oauth-authorization-server/api/auth
- **Client registration:** Client ID Metadata Documents (an HTTPS URL as client_id), or Dynamic Client Registration (RFC 7591). No pre-registration needed.
- **Tokens:** Bearer access tokens in the Authorization header (never in the URL), with refresh tokens through offline_access. Access stops at once when the user disconnects the app in Settings → Connected apps.

### Scopes

- `leads:read`: Read searches, leads, contacts and account. Required by every tool except start_search.
- `leads:search`: Run searches, which spend credits. Required by start_search.

## Rate limits

Per user, across all connected apps. Over a tool limit, the call returns RATE_LIMITED with the seconds to wait.

- 60 / min: All tool calls
- 10 / min: start_search, and 100 a day
- 300 / hour: get_lead_contacts
- 120 / min: HTTP requests to /mcp

## Tools (9)

Only start_search spends anything; every other tool reads the account and is free.

| Tool | What it does | Parameters | Returns | Scope | Cost |
| --- | --- | --- | --- | --- | --- |
| `get_account` | Returns your plan, credit balance and, on the free plan, how many free searches are left today. | None | plan, credits_balance, free_search (limit, used, remaining, resets_at), can_use_extended_search, pricing_url | leads:read | Free |
| `list_supported_free_searches` | Lists the business type + city pairs the free plan can search. | country (ISO-2, optional), business_type (optional filter) | combos (business_type, location, country), total | leads:read | Free |
| `estimate_search` | Shows what a search would cost and whether your account can run it, without starting it or charging anything. | Same as start_search, without idempotency_key | mode (free \| paid), cost_min, cost_max, credits_balance, can_run, message | leads:read | Free |
| `start_search` | Starts a search for local businesses with no website or a weak one. Returns a search_id right away. | business_type, country, location (required on Free), website_status (no_website \| weak_website \| both), contacts_filter, depth (standard \| extended), idempotency_key | search_id, status (pending), mode, credits_charged, credits_balance, free_search_remaining, next_step | leads:search | Free plan: the daily free search. Paid plans: 2–2.5 credits (standard) or 7.3–8.8 credits (extended, Pro and Scale) |
| `get_search` | Returns the status of a search and the leads found so far, best opportunities first. Waits up to 25 seconds for it to finish. | search_id, wait_seconds (0–25), page, per_page (up to 25), include_images | status (pending \| collecting \| analyzing \| done \| done_empty), query, progress, a page of leads ranked by opportunity, total_leads, next_page, credits_charged, credits_refunded, poll_again_after_seconds | leads:read | Free |
| `list_searches` | Lists your lead searches, newest first. | page, per_page (up to 25) | searches (search_id, business_type, location, status, lead_count, mode, created_at), total, next_page | leads:read | Free |
| `list_leads` | Lists your saved leads across all searches, or the leads of one search ranked by opportunity. | search_id, status, has_email, query, page, per_page (up to 25) | leads (business, city, website, problem, top issue, opportunity score, rating), total, page, next_page | leads:read | Free |
| `get_lead` | One lead's full card: what is wrong with its website, the issues worth pitching, screenshots and any drafted outreach. | lead_id, include_images | the lead card plus explanation, pitchable_signals, other_signals, screenshots, existing_drafts, has_built_site | leads:read | Free |
| `get_lead_contacts` | One lead's phone, email, address and social profiles, as shown in the app. | lead_id | phone, email, address, hours, contact_page_url, socials, website_url, maps_url, app_url | leads:read | Free |

## Errors

Auth and transport failures are HTTP status codes. Everything else is a tool result with isError: true and a body with code, message, retryable and request_id.

| Where | Code | Meaning |
| --- | --- | --- |
| 401 | `invalid_token` | Missing, expired or revoked token, or the app was disconnected. The client should start the OAuth flow again. |
| 403 | `insufficient_scope` | The token lacks a scope the call needs, for example leads:search for start_search. |
| 403 | `account_suspended` | The LeadReef account is suspended. |
| 429 | `rate_limited` | Over the HTTP request limit. Retry-After gives the seconds to wait. |
| tool | `VALIDATION_ERROR` | A parameter is missing or out of range. The message names the problem. |
| tool | `NOT_FOUND` | The search_id or lead_id does not exist or belongs to another account. |
| tool | `INSUFFICIENT_CREDITS` | Not enough credits for this search. Nothing was charged. Check with estimate_search first. |
| tool | `FREE_QUOTA_EXHAUSTED` | The free plan's daily search is already used. resets_at says when the next one is available (midnight US Eastern). |
| tool | `PLAN_REQUIRED` | The free plan only searches the pairs from list_supported_free_searches, and depth: extended needs Pro or Scale. required_plan names the plan. |
| tool | `DISPOSABLE_EMAIL` | Free searches are not available on accounts with a disposable email address. |
| tool | `RATE_LIMITED` | Over a tool rate limit. retry_after_seconds gives the seconds to wait. |
| tool | `SEARCH_FAILED` | The search failed. Any credits it charged were returned; starting a new search may work. |
| tool | `TEMPORARILY_UNAVAILABLE` | LeadReef is briefly unavailable. Retry in a moment. |
| tool | `INTERNAL` | Unexpected error on LeadReef's side. Retry once. |

Source: [leadreef.ai/mcp/docs](https://leadreef.ai/mcp/docs). Changes: [CHANGELOG.md](../CHANGELOG.md).
