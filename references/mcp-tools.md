# MCP Tool Reference for Research

## Discovery & Search
| Tool | Best For | Notes |
|------|---------|-------|
| `web_search_exa` | Semantic discovery | **Start here.** Finds what keyword search misses. |
| `web_search_advanced_exa` | Date/category filtered | Recency checks, targeting content types. |
| `firecrawl_search` | Search + scraping | Search results and content in one step. |
| `brave_web_search` | Independent index | Different index from Google. |
| `brave_news_search` | News with freshness | Current events, recent developments. |
| `tavily_search` / `tavily_qna` | Fast fact verification | Specific fact-checks, not broad discovery. |

## Academic & Scholarly
| Tool | Best For | Notes |
|------|---------|-------|
| Paper Search | Multi-database paper search | Primary academic search. |
| OpenAlex | Bibliometrics, trends, 477M+ works | Research landscape mapping. Semantic search available. |
| Semantic Scholar | Citation graph, author networks | Map how papers connect. |
| Crossref | DOI resolution, verification | **Use before citing any paper.** |
| Unpaywall | Open-access full-text PDFs | Need paper, not just abstract. |

## Quality Signals (optional — enhance when available)
| Tool | Best For | Notes |
|------|---------|-------|
| Scite | Citation context (supporting/disputing/mentioning) | 85% precision, 45% recall. Trust "disputing" flags. Do NOT trust absence of disputes. Key sources only — not a discovery channel. |
| Consensus | Study design filtering (RCT vs. observational) | Adds study-design-aware quality signals. Not a discovery channel. Free/no-auth. |

**Degradation:** If Scite/Consensus unavailable, note in source log and rely on manual quality signal assessment (green/red flags in source-hierarchy.md). The system functions without them — they enhance, not gate.

## Content Extraction
| Tool | Best For | Notes |
|------|---------|-------|
| `firecrawl_scrape` | Full-page, JS, anti-bot | Most capable scraper. |
| `crawling_exa` | URL extraction | Lighter Firecrawl alternative. |
| `web_fetch` | Simple page fetching | Standard HTML pages. |

## Search Strategy — Semantic-First Ordering

**Step 1: Semantic Discovery (3-5 searches)** — Cast a wide net. Start with Exa, add Brave/Firecrawl for breadth. Vary search terms.

**Step 2: Academic Evidence (3-5 searches)** — Don't skip even for practical topics. Paper Search, OpenAlex, Semantic Scholar.

**Step 3: Citation Chaining (per spiral pass, 3-round ceiling)** — Backward then forward snowballing. Zero-new-inclusions stop per TARCiS R7. Ceiling is per-pass, not global.

**Step 4: Quality Signals (if tools available)** — Scite on key sources, Consensus for study design. Skip if unavailable.

**Step 5: Contrarian Search Round (ALWAYS runs)** — 2-3 explicit counter-evidence queries. Not optional. Scite "disputing" citations as seeds if available.

**Step 6: Verification & Full-Text Access** — Crossref for DOI verification, Unpaywall for full text, Firecrawl for paywalled content.

## Stopping Rules

Stop when 2+ of these signals appear:
- New searches return already-found sources (saturation)
- Last 3 searches added no new substantive claims
- 3+ independent sources for every major claim
- All major sub-questions from Phase 0 covered
- Counter-evidence searched for every major claim

**These also serve as spiral loop-back triggers when NOT met.**

**Do not stop before** 3 different search channels and 8 searches total (deep) or 5 searches (quick).
