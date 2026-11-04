---
name: complaints-miner
description: Mine real user complaints from Reddit, Hacker News and X about a set of competitors (mode=competitor) or a problem space (mode=space), cluster them, and return the top 10 complaints with verbatim quotes and links. Use when preparing customer-discovery calls, checking an idea against evidence, or scanning a space for problems worth solving.
---

# Complaints miner

Find what people actually complain about. No guessing, no synthesis without quotes.

## Inputs

Ask for these if missing:

- `mode`: `competitor` or `space`
- `targets`:
  - competitor mode → 1–5 product names (e.g. "Devin", "Unblocked")
  - space mode → 1–3 phrases describing a role or workflow (e.g. "engineering managers onboarding new hires")
- `window` (optional): default last 12 months
- `output` (optional): `doc` (default) or `chat`

## Tools

Use whatever is connected, in this order of preference:

1. **Reddit**: Bright Data `search_scrapers` → reddit posts/comments scraper, or `search_engine` with `site:reddit.com` style queries, then `scrape_as_markdown` on the threads.
2. **Hacker News + X + blogs**: Exa `web_search_exa` (semantic; describe the page you want: "people complaining that Devin ...").
3. Fallback: WebSearch + WebFetch.

Never use a tool the user has not connected. Do not install anything.

## Procedure

1. **Build queries.** For each target, 6–10 queries mixing: `<target> frustrating`, `<target> doesn't`, `switched from <target>`, `<target> vs`, `<target> waste`, `anyone else <target>`, and in space mode: `<space> I hate`, `<space> wish there was`, `<space> spend hours`, `<space> manual`.
2. **Collect.** Aim for 60–150 raw snippets. Keep: text, author handle (if public), date, URL, source (reddit/hn/x/blog), upvotes/likes if visible.
3. **Filter.** Drop: marketing, vendor posts, bots, duplicates, anything older than `window`.
4. **Cluster.** Group snippets into complaints. A complaint is one sentence in the user's words, e.g. "It loses context after a few files and starts guessing."
5. **Rank.** Primary: number of distinct people saying it. Secondary: upvotes. Tie-break: presence of money/time signals ("we pay", "hours a week", "cancelled").
6. **Quote.** For each of the top 10, attach 2–3 verbatim quotes (≤ 40 words each) with link and date. If a complaint has fewer than 2 quotes from different people, it does not make the list.
7. **Flag pay signals.** Mark complaints where anyone mentions paying, switching, or quitting a tool over it.

## Output format

Title: `Complaints — <targets> — <date>`

Then exactly this table, nothing above it except a 2-line summary (how many snippets, from where):

| # | Complaint (user's words) | People | Pay signal | Quotes |
|---|---|---|---|---|
| 1 | ... | 7 | yes | "…" — u/name, Reddit, 2026-09 (link) · "…" — … |

After the table:

- **Not found**: things you searched for and saw no complaints about (so the user knows the absence is real).
- **Sources searched**: list of queries run and platforms hit.

If `output=doc`, write to the user's Drive folder they name, else return in chat.

## Rules

- Quotes are verbatim. Never paraphrase inside quotation marks.
- No complaint without a link.
- Do not add your own opinion of which complaint is "the best opportunity". The user decides.
- If fewer than 5 complaints clear the 2-person bar, say so and stop; a thin result is a finding.
