# Security

## Reporting a vulnerability

Email **support@leadreef.ai** with the subject line "Security". Please do not open a public issue for a security problem.

Include what you found, how to reproduce it, and what an attacker could do with it.

## How access works

- The server is hosted by LeadReef at `https://api.leadreef.ai/mcp`. This repository holds documentation and client configs only.
- Sign-in uses OAuth 2.1 with PKCE (S256). The user signs in on leadreef.ai and approves the app there. The client never sees the password.
- Clients register with Client ID Metadata Documents or Dynamic Client Registration (RFC 7591).
- Access tokens go in the `Authorization: Bearer` header, never in the URL.
- Scopes: `leads:read` (read searches, leads, contacts and the account) and `leads:search` (run searches, which spend credits).
- A connected app cannot send messages, delete leads, change billing or publish sites.
- Users can revoke any app in Settings → Connected apps. Access ends immediately.

OAuth metadata:

- Protected resource: https://api.leadreef.ai/.well-known/oauth-protected-resource/mcp
- Authorization server: https://api.leadreef.ai/.well-known/oauth-authorization-server/api/auth
