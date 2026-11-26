---
name: find-leads
description: Find local businesses with no website or a weak one through the LeadReef MCP server, review the best leads, pull contacts and write the pitch. Use when the user asks for leads, prospects or businesses without a website in a city.
---

# Find leads with LeadReef

Use this skill when the user wants local businesses to pitch a website to: "find plumbers in Denver with no website", "who should I pitch in Lisbon", "show my leads", "write an email to the top three".

It needs the LeadReef MCP server (`https://api.leadreef.ai/mcp`). If the tools are missing, tell the user to connect it: https://leadreef.ai/mcp.

## Before a new search

1. Call `get_account` to see the plan, credit balance and free searches left today.
2. On the Free plan, call `list_supported_free_searches` and check the business type and city are on the list. If not, offer the closest pair from the list or mention that paid plans search any city.
3. Call `estimate_search` with the same parameters you plan to use. Tell the user what it costs and what is left after it.
4. Ask the user to confirm before `start_search`, unless they already said to go ahead. A search spends credits or the daily free search; nothing else does.

Check saved leads first: if the user may already have a search for that city, call `list_searches` and offer to reuse it. Reading saved leads is free.

## Running the search

1. Call `start_search` with `business_type`, `country` and `location`. Use `website_status` to match the request: `no_website`, `weak_website` or `both`. `sources` picks where to look (`maps`, `recent_ads`, `open_web`); Recent Ads and Open Web need Pro or Scale and cost more, so mention it in the estimate. Pass an `idempotency_key` so a retry does not start a second search.
2. Call `get_search` with the returned `search_id` and `wait_seconds: 25`. Repeat until `status` is `done` or `done_empty`, waiting `poll_again_after_seconds` between calls. Tell the user it is running; do not start another search meanwhile.
3. If it ends `done_empty`, say so and suggest a nearby city, a related business type or `website_status: both`.

## Presenting leads

- Show a short table: business, city, website or "no website", the main problem, rating. Keep the order LeadReef returns; it puts the best opportunities first.
- Offer the next page instead of dumping every lead.
- To narrow a long list, pass filters to `get_search` or `list_leads`: `has_email`, `has_phone`, `problem`, `min_score`, `query`.
- Call `get_lead` only for the leads the user picks, and `get_lead_contacts` only when they want to reach out.

## Writing the pitch

- Use only facts from `get_lead` and `get_lead_contacts`: the business name, what is wrong with its site or that it has none, its rating and reviews. Never invent a problem, a number or a contact.
- Keep a first email under 90 words, name one specific thing, and end with a question.
- LeadReef and this skill never send anything. Hand the draft to the user to send.

## Errors

- `INSUFFICIENT_CREDITS`: nothing was charged. Tell the user their balance and link https://leadreef.ai/pricing.
- `FREE_QUOTA_EXHAUSTED`: the daily free search is used. Give `resets_at` and offer to work with saved leads.
- `PLAN_REQUIRED`: the request needs a paid plan or a supported free pair. Name `required_plan`.
- `RATE_LIMITED`: wait `retry_after_seconds`, then retry once.
- `SEARCH_FAILED`: any credits were returned. Offer to start the search again.
- `401 invalid_token`: ask the user to reconnect LeadReef in their app.
