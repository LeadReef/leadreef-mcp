# Example prompts

Each prompt works in any connected app. The tools listed are the ones the AI usually calls; only `start_search` spends anything.

## Find new leads

**"Find 20 dentists in Austin with no website and rank them by how much they need one."**
`get_account` → `estimate_search` → `start_search` → `get_search`. Costs 1 search.

**"Find hair salons in Denver whose website is weak, not missing."**
Same flow with `website_status: weak_website`. Costs 1 search.

**"I'm on the free plan. What can I search today?"**
`get_account` → `list_supported_free_searches`. Free.

**"How much would a search for roofers in Phoenix cost with Maps, Recent Ads and Open Web?"**
`estimate_search` with `sources: ["maps", "recent_ads", "open_web"]`. Free; nothing is started.

## Work your saved leads

**"From my Austin search, show leads with an email and a score above 70."**
`list_leads` with `has_email` and `min_score`. Free.

**"Show my leads from the Lisbon search that have a phone number."**
`list_searches` → `list_leads`. Free.

**"Which of my saved leads have 4.5+ stars on Google but no site?"**
`list_leads`. Free.

**"What is wrong with Oak Street Dental's website?"**
`list_leads` (query) → `get_lead`. Free.

## Reach out

**"Get the email and phone for Bella Vista Trattoria."**
`get_lead_contacts`. Free.

**"Take the top 5 restaurants from my Manchester search and write each a 3-line cold email."**
`list_leads` → `get_lead` → `get_lead_contacts`, then the AI writes the drafts. Free. LeadReef never sends them; you do.

## Account

**"How many credits do I have left this month?"**
`get_account`. Free.
