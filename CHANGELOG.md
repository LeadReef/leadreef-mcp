# Changelog

## 1.1.0 — 2026-11-20

- `start_search` and `estimate_search` take `sources`: Maps, Recent Ads and Open Web, in any combination.
- `get_search` and `list_leads` filter leads by text, problem, email, phone and minimum score. `list_searches` filters by status, text and free or paid.
- `start_search` is marked destructive because it spends credits, so clients ask before each paid search.
- Refresh tokens are advertised through `offline_access`.
- Breaking: unknown arguments are rejected; plan errors return `plans_url`; `list_leads` drops the `status` filter; `get_lead` returns each pitchable issue as `{ issue, pitch }`.

## 1.0.0 — 2026-11-06

First public release of the LeadReef MCP server.

- Nine tools: get_account, list_supported_free_searches, estimate_search, start_search, get_search, list_searches, list_leads, get_lead, get_lead_contacts.
- OAuth 2.1 with PKCE, Client ID Metadata Documents and Dynamic Client Registration.
- Streamable HTTP transport, MCP 2025-11-25 (2025-06-18, 2025-03-26 and 2024-11-05 also accepted).
