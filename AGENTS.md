# LeadReef

The `leadreef` MCP server finds local businesses with no website or a weak one and returns their contacts.

- Connect the server at https://api.leadreef.ai/mcp and sign in when asked (Codex: `codex mcp login leadreef`).
- Only `start_search` spends anything (credits, or the daily free search on the Free plan). Every other tool is free.
- Before `start_search`, call `get_account` and `estimate_search`, tell the user the cost and wait for a yes.
- After `start_search`, call `get_search` with `wait_seconds: 25` until the status is `done` or `done_empty`.
- Write pitches only from facts returned by `get_lead` and `get_lead_contacts`. LeadReef never sends messages; the user sends them.

Full workflow: skills/find-leads/SKILL.md. Tool reference: https://leadreef.ai/mcp/docs
