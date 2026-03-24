---
name: research
description: >
  Use when conducting research at any depth — full deep dives, quick briefs,
  source lists, contrarian analysis, domain maps, or tactical playbooks.
  Convergence-driven spiral process with claim verification pipeline,
  GRADE-inspired confidence domains, and adversarial review.
  Triggers on 'research this topic', 'deep dive into', 'quick brief on',
  'find sources about', 'contrarian view on', 'map this domain',
  'what are the best tactics for', 'update my research on', 'harvest content from'.
user-invokable: true
args:
  - name: mode
    description: "deep | quick | sources | contrarian | map | tactics | update | harvest | audit | status"
    required: false
  - name: topic
    description: "Topic, question, or path (for update/harvest/audit)"
    required: false
tags: [deep-research, briefs, source-lists, contrarian-analysis]
cluster: content-research
related: [research-teach, research-audit, intel]
---

# Research Cascade

## Purpose

Unified research skill with 10 modes from quick briefs to full deep dives. Every mode follows the same evidence standards — source tiers, domain-decomposed confidence, triangulation, and claim verification pipeline. Your job is to be right, not comprehensive.

## Mode Routing

| Mode | What it does | Depth | Verification Level |
|------|-------------|-------|--------------------|
| `deep` (default) | Full spiral research, 4,000-8,000 words | All phases | Standard (upgradable to Rigorous) |
| `quick` | Research brief, ~1,500-2,500 words | Condensed | Light |
| `sources` | Annotated source list by tier, no synthesis | Discovery only | Light |
| `contrarian` | Steelman minority/contrarian views | Layer 4 focus | Standard |
| `map` | Domain map: researchers, institutions, debates | Orientation only | None |
| `tactics` | Actionable tactics only, evidence-grounded | Layer 3 focus | Light |
| `update` | Revisit existing file with new evidence | Targeted | Standard |
| `harvest` | Scan research doc for content elements | Extraction | None |
| `audit` | Re-run verification on existing file | Verification | Standard |
| `status` | List all research files with staleness | Inventory | None |

For `update`, `harvest`, `audit`, `status` — skip brainstorm gate, execute directly.

## Vault + QMD Dedup Check

Before starting research, check for prior work:

1. Check vault: `ls ~/Vault/ai/*/artifacts/` for existing research on this topic
2. Query QMD for prior research sessions:
   ```bash
   qmd query "[research-topic]" -c session-insights -n 5
   ```
3. Check vault human side for Sam's existing thinking:
   ```bash
   qmd query "[research-topic]" -c vault -n 5
   ```
4. If prior work found, note it and ask whether to build on it or start fresh

**Discovery:** On local machine, prefer `obsidian search` and `obsidian backlinks`. On VPS, use QMD queries + direct file reads.

**Degradation:** If vault unavailable, skip dedup and proceed. Warn about potential duplicated work.

## Setup (All Modes)

1. Read `~/Vault/ai/sam.md` for user context
2. Check `/research/outputs/` for overlapping research

## Smart Start: Brainstorm Gate

Before routing to any research mode (except update/harvest/audit/status):

1. **Restate the question** — confirm understanding
2. **Sharpen the angle** — suggest tighter framing if needed
3. **Surface assumptions** — what might the user not have considered?
4. **Propose scope** — recommend a mode if unspecified

---

## Spiral Process Architecture

The research process is a convergence-driven spiral, not a linear pipeline. Each pass refines the output; the spiral terminates when quality triggers stop firing.

### State Machine

```
ORIENT → GATHER → SYNTHESIZE → VERIFY → AUDIT → [LOOP OR PROCEED] → OUTPUT
```

**Transitions:**

```
IF pass == 1:
    ORIENT → GATHER → SYNTHESIZE → VERIFY(lightweight) → AUDIT
    Artifact: Draft with inline confidence labels
    → Evaluate loop-back triggers

IF pass == 2:
    Re-execute only phases with active triggers
    Artifact: Gap/conflict list + revised draft
    → Evaluate loop-back triggers

IF pass >= 3:
    Targeted resolution of remaining gaps
    Artifact: Final draft with full verification
    → Evaluate loop-back triggers

PROCEED WHEN: fewer than 2 triggers active
ALSO PROCEED WHEN: hard cap of 4 passes reached
ALSO PROCEED WHEN: quality score declined between passes (diminishing returns)
```

### Loop-Back Triggers

Evaluate after each pass. Each trigger is either active or resolved:

| # | Trigger | Active when | Resolution |
|---|---------|-------------|------------|
| 1 | **Confidence gap** | Any claim rated Emerging or Speculative on a question the user needs answered | Find stronger evidence or explicitly note the gap |
| 2 | **Coverage gap** | Known-item test fails — a top-cited paper appears in only one search channel | Run additional search channels for missing coverage |
| 3 | **Triangulation failure** | A major claim has fewer than 3 independent Tier B+ sources | Search for additional independent sources |
| 4 | **Contrarian gap** | No counter-evidence searched for a major claim | Run explicit disconfirmation searches |
| 5 | **Source staleness** | Key evidence is >5 years old with no replication check | Search for recent replications or updates |

**Proceed when fewer than 2 triggers remain active.** Don't chase perfection — proceed when quality is sufficient for the decision at hand.

### Verification Depth by Pass

| Pass | Verification | Confidence |
|------|-------------|------------|
| 1 | Lightweight: citation existence + headline spot-checks | Simplified labels |
| 2 | Lightweight: focus on gap-list claims | Update labels for changed claims |
| Final | Full verification at selected level | Full domain assessment |

---

## Phase 0: Orientation

### 0a. Check Prior Art
Search output directories for overlapping research. Reference, don't duplicate.

### 0b. Map the Territory
Identify the domain, question type (empirical/theoretical/practical/strategic), action context, and priors. Output a brief Domain Map (3-8 sentences).

### 0c. Calibrate Depth
Deep = all layers + full verification, 4,000-8,000 words.

### 0d. Select Verification Level

| Level | When | Decomposition | Citation Check | Confidence | Approximate Cost |
|-------|------|--------------|----------------|------------|-----------------|
| **Rigorous** | High-stakes decisions, client-facing | Atomic for all claims | Existence + metadata + semantic support | Full 4-domain | ~370 verification calls, ~25 min |
| **Standard** | Default for deep research | Sentence-level; atomic for high-risk claims | Existence all + semantic for high-risk | Full 4-domain | ~98 calls, ~10 min |
| **Light** | Quick briefs, tactical, sources | Skip decomposition | Existence for key citations | Simplified single-dimension | ~30 calls, ~5 min |

Deep research defaults to Standard. User can upgrade to Rigorous. Quick/tactical/sources default to Light.

---

## Source Discovery Engine

Source hierarchy and quality signals in `references/source-hierarchy.md`. MCP tool reference in `references/mcp-tools.md`.

### Search Strategy — Semantic-First Ordering

**Step 1: Semantic Discovery (3-5 searches)**
Start with Exa (`web_search_exa`). Finds what keyword search misses. Add Brave/Firecrawl for breadth. Vary search terms.

**Step 2: Academic Evidence (3-5 searches)**
Don't skip even for practical topics. Paper Search, OpenAlex (477M+ works), Semantic Scholar. Most practical claims have an evidence base or conspicuously lack one.

**Step 3: Citation Chaining (per pass, 3-round ceiling)**
After initial discovery, run backward then forward snowballing:
- Round 1: Backward snowball — what do key papers cite?
- Round 2: Forward snowball — what cites key papers?
- Round 3: If new inclusions found, one more round. Stop at zero new inclusions per TARCiS R7.
- Ceiling is per-pass, not global. Each spiral pass gets its own 3 rounds.

**Step 4: Quality Signals**
- **Scite** (if available): Run on key sources only. Trust "disputing" flags (85% precision). Do NOT trust absence of disputes (45% recall). Use disputing citations as seeds for contrarian search.
- **Consensus** (if available): Study design filter — adds RCT vs. observational vs. review signals. Not a discovery channel.
- If Scite/Consensus unavailable: note in source log, rely on manual quality signal assessment (green/red flags in `references/source-hierarchy.md`).

**Step 5: Contrarian Search Round (ALWAYS runs)**
2-3 explicit counter-evidence queries. Not optional. Not conditional on Scite. This is a structural guarantee against confirmation bias.
- Query patterns: "[topic] criticism", "[topic] failed", "[topic] limitations", "[main claim] evidence against"
- If Scite available, use "disputing" citations as additional seeds.

**Step 6: Verification & Full-Text Access**
Crossref for DOI verification. Unpaywall for full text. Firecrawl for paywalled content. **Use Crossref before citing any paper.**

### Coverage Verification

After Pass 1, run retroactive known-item testing:
1. Identify 3-5 most-cited papers from gathered sources
2. Verify each appears across multiple search channels
3. If any top paper found via only one channel → flag **coverage gap** (triggers spiral Pass 2)

### Gathering Rules

- Search broadly first (Steps 1-2), then deeply (Steps 3-6)
- Prioritize original sources — chase citations via Crossref, read via Unpaywall/Firecrawl
- Seek disconfirmation for every major claim (not optional — Step 5)
- Time-test filter: sources >5 years old need replication check
- Vary tools: at least 3 different search channels per topic
- Minimum: 8 searches total, 3 channels. Complex topics: 15-20 searches.

### Stopping Rules

Stop when 2+ of these signals appear:
- New searches return already-found sources (saturation)
- Last 3 searches added no new substantive claims
- 3+ independent sources for every major claim
- All major sub-questions from Phase 0 covered
- Counter-evidence searched for every major claim

**These also serve as spiral loop-back triggers when NOT met.**

---

## Analysis & Synthesis

Runs within each spiral pass. Structure across 5 layers:

**Layer 1: Foundations (80/20)** — 3-7 principles explaining 80% of outcomes. Each with mechanism, example, confidence label.

**Layer 2: Current Evidence Landscape** — Organized by sub-question, not by source. What evidence shows, how strong, where it conflicts.

**Layer 3: Practical Tactics** — Concrete enough to execute. Tied to evidence. Labeled for difficulty, impact, confidence.

**Layer 4: Contrarian & Minority Views** — Replication concerns, practitioner-academic gaps, emerging counter-evidence, structural critiques. Not bothsidesism — steelman each position.

**Layer 5: "So What?" — Decision Translation** — What changes in behavior? Cost of ignoring? Cost of acting if wrong?

---

## Claim Verification Pipeline

Post-synthesis verification layer. Runs after each spiral pass — lightweight in early passes, full in the final pass.

### Claim Decomposition

| Level | Method | When |
|-------|--------|------|
| **Light** | Skip decomposition — verify key claims only | Quick/tactical modes |
| **Standard** | VeriScore sentence-level (~1.6 verifiable claims per sentence) | Default for deep |
| **Rigorous** | Atomic decomposition for all claims | High-stakes research |

**Escalate to atomic** for: quantitative claims, headline findings, anything rated Strong confidence.

**Always decontextualize** before verification: resolve references ("this study" → actual study name, "the researchers" → named authors) so each claim is self-contained.

### Evidence-First Verification Protocol

For each extracted claim:

```
1. Generate search query from the claim (NOT from the draft)
2. Retrieve evidence independently
3. Compare retrieved evidence to the claim
4. Render verdict: Verified | Unverified | Disputed | Irrelevant
```

**Critical:** Do NOT re-read the draft while verifying. Evidence-first means retrieve evidence, THEN compare. Same-context verification confirms errors instead of catching them (CoVe, Dhuliawala et al., ACL Findings 2024).

### Citation Verification — Three Layers

| Layer | What | When | How |
|-------|------|------|-----|
| 1. Existence | Does this paper/source actually exist? | All levels | Crossref DOI lookup |
| 2. Metadata | Do author, year, journal match? | Standard+ for high-risk | Crossref + Semantic Scholar |
| 3. Semantic support | Does the source actually say what the claim says? | Rigorous; Standard for high-risk | Retrieve source, compare to claim |

### Unverifiable Claim Classification

Do not discard unverifiable claims — classify them by reason:

| Situation | Classification | Treatment |
|-----------|---------------|-----------|
| No source found | `[No supporting source found]` | Prominent flag, reader decides |
| Source doesn't clearly support | Weaken claim to match what source actually says | Revise inline |
| Author synthesis (connecting verified facts) | `[Author synthesis]` | Retain with provenance — show which verified facts it connects |
| Training-data recall, unverified via search | `[From training data — not verified]` | Emerging ceiling, cannot be rated Strong |

### Verification in the Spiral

- **Early passes:** Lightweight — citation existence + headline spot-checks. Feeds gap list for next pass.
- **Final pass:** Full verification at selected level. Produces final verdicts.
- **Verification failures trigger loop-back:** unverified high-confidence claims → confidence gap trigger.

---

## Confidence Framework

Replaces star ratings with GRADE-inspired domain-decomposed confidence. Each claim is assessed across independent domains so the reader knows *why* it's uncertain, not just *how much*.

### 4 Core Domains

Assess each domain independently per claim. Do NOT let a strong impression in one domain inflate others.

| Domain | What it measures | Key question |
|--------|-----------------|--------------|
| **Evidence strength** | Source tier, sample size, replication status | How strong is the underlying evidence? |
| **Consistency** | Agreement across independent sources | Do independent sources agree? |
| **Directness** | Whether evidence addresses the actual question asked | Does this evidence answer the user's question, or an adjacent one? |
| **Synthesis integrity** | Whether connections between verified facts are logical | Are the inferences sound, or is this a leap? |

### 2 Optional Flags

Appear only when relevant:

| Flag | When to apply |
|------|--------------|
| ⚠ **Precision concern** | Quantitative claims without confidence intervals, small samples, suspiciously round numbers |
| ⚠ **Completeness concern** | Suspected missing studies, file-drawer problem, search only in English |

### Overall Labels

Derived from domain profiles, not assigned independently:

| Label | Profile | Inline format |
|-------|---------|---------------|
| **Strong** | All 4 domains solid, no flags | `[Strong — direct, consistent, multi-source]` |
| **Moderate** | 1 domain weak or 1 flag active | `[Moderate — strong evidence, indirect]` |
| **Emerging** | 2+ domains weak, limited evidence | `[Emerging — single source, direct, logical synthesis]` |
| **Speculative** | Thin evidence, extrapolation, unverified | `[Speculative — extrapolation from adjacent field]` |

At **Light verification level:** Use simplified labels only (Strong/Moderate/Emerging/Speculative) without domain breakdown. No confidence appendix.

At **Standard+ verification level:** Full 4-domain assessment. Inline labels + confidence appendix in output.

### Calibration Discipline

- **Reference class anchoring:** "What's the base rate for claims like this being true?" Consider the outside view before assessing.
- **Internal consistency:** Same evidence type → same confidence across claims. If two claims rest on similar evidence, they get similar ratings.
- **When in doubt, round DOWN.** Overconfidence is the most common error.
- Two credible sources contradict → Moderate ceiling for the contested claim.
- Can't find original source → Emerging ceiling regardless of plausibility.

---

## Adversarial Phase

Runs in the **final spiral pass only**, after synthesis and verification. Replaces the linear self-audit with a structured adversarial review.

### Step 1: Premortem

> "Assume this research has been proven wrong in 12 months. Write the 3 most likely reasons why."

Do not defend the research — attack it. Use prospective hindsight to surface vulnerabilities that forward-looking analysis misses.

### Step 2: Targeted Critique

For each vulnerability the premortem surfaced, examine the specific claims involved:
- Is the evidence as strong as presented?
- Is there counter-evidence not addressed?
- Is the confidence label justified?
- Would a domain expert find this claim defensible?

### Step 3: Resolution

For each vulnerability, take one action:
- **Strengthen** — find additional supporting evidence
- **Weaken** — downgrade the confidence label
- **Caveat** — add an explicit limitation note
- **Note** — document the limitation in Audit Notes

If the premortem changed nothing, explain why: genuinely robust, or was the critique shallow?

### Inherited Checks

Preserve from the original audit — verify these are addressed:
- False precision — are numbers real or suspiciously specific?
- Circular reasoning — do multiple claims trace back to one source?
- Absence vs. evidence — "I didn't find proof" ≠ "it's not true"

### Output

Audit Notes section with:
- Premortem results (3 vulnerabilities + resolution)
- Verification summary (claims verified/unverified/disputed)
- Inherited check results
- Known limitations

---

## Shelf-Life Markers

| Marker | Time Horizon | Format |
|--------|-------------|--------|
| 🟢 **Durable** | 3-5 years | `🟢 Durable — review if [observable signpost]` |
| 🟡 **Monitor** | 1-3 years | `🟡 Monitor — review if [observable signpost]` |
| 🔴 **Perishable** | <12 months | `🔴 Perishable — review if [observable signpost]` |

Each marker includes an **observable signpost** — a specific event that should trigger review:
- `🟢 Durable — review if GRADE methodology undergoes major revision`
- `🟡 Monitor — review if new RCT data published in this domain`
- `🔴 Perishable — review if competitor launches competing product`

**Update triggers (hybrid):** Time-based default (check at shelf-life expiry) + event-based signposts (check immediately when signpost fires).

---

## Output Templates

### Deep Research

```markdown
# [Research Topic]
_Research conducted: [date] | Verification: [Rigorous/Standard/Light] | Overall shelf-life: [🟢/🟡/🔴]_
_Spiral: [N] passes | Claims verified: [X/Y] | Triggers resolved: [list]_

## Domain Map
## Executive Summary
## 1. Foundations — The 80/20
## 2. Current Evidence Landscape
## 3. Practical Tactics
## 4. Contrarian & Minority Views
## 5. Decision Translation — "So What?"
## Key Unknowns & Open Questions
## Source Log
## Audit Notes
## Confidence Appendix
```

**Confidence Appendix** (Standard+ only): Full domain breakdown for every claim rated in the document.

```markdown
## Confidence Appendix

| Claim | Evidence | Consistency | Directness | Synthesis | Flags | Overall |
|-------|----------|-------------|------------|-----------|-------|---------|
| "Market is growing at 15% CAGR" | Strong (Tier A, replicated) | Strong (3 independent) | Strong (direct) | N/A (factual) | ⚠ Precision | Strong |
| "This trend will accelerate" | Emerging (1 study) | N/A (single source) | Moderate (adjacent) | Moderate (logical) | — | Emerging |
```

**Source Log** — include for each source:
- Citation, tier, quality signals (green/red flags)
- Search channel(s) that found it
- Scite signals if available (supporting/disputing/mentioning count)

Save to `~/Vault/ai/[project]/artifacts/[topic-slug].md`. If no vault project, fall back to `/research/outputs/[topic-slug].md`.

### Quick Brief

```markdown
# [Topic] — Brief
_Research conducted: [date] | Verification: Light | Shelf-life: [🟢/🟡/🔴]_

## Domain Map (3-5 sentences)
## Top Findings (3-5, with simplified confidence labels)
## Tactics (3-5, with difficulty/impact)
## Key Unknowns
## Sources
```

Minimum 5-8 searches, 2+ channels. Save as `[topic-slug]-brief.md`.

---

## Other Modes

**Sources:** Full search strategy, no synthesis. Each source gets: citation, summary, quality signals, tier, search channel. Save as `[topic-slug]-sources.md`.

**Contrarian:** Focus on Layer 4: replication concerns, practitioner-academic gaps, emerging counter-evidence, structural critiques. Steelman each. Standard verification on key contrarian claims. Save as `[topic-slug]-contrarian.md`.

**Map:** Phase 0b only: key researchers, institutions, schools of thought, foundational texts, major debates, field trajectory. 500-1,000 words. Save as `[topic-slug]-map.md`.

**Tactics:** Layer 3 only: concrete enough to execute, tied to evidence, labeled for difficulty/impact/confidence. Light verification. Save as `[topic-slug]-tactics.md`.

**Update:** Read existing file, note date/shelf-life, search for new evidence since publication, run verification on changed claims, append Update section with findings and changed confidence levels.

**Harvest:** Scan research doc for extractable content elements across 6 tag types: `[STAT]`, `[QUOTE]`, `[FRAMEWORK]`, `[STORY]`, `[CONTRARIAN]`, `[TACTIC]`. Present harvest map with candidate hooks and format recommendations. Gate to `/content draft`.

**Status:** List all research files with dates and shelf-life markers. Flag stale files. If content pipeline exists, show pipeline state grouped by status.

---

<important>
## Hard Gates

1. **Never cite a study you haven't found via search in this session.** Training-data recalls get `[From training data — not verified]` and Emerging ceiling.
2. **Never invent author names, journal names, years, or sample sizes.** Say "approximately" or omit.
3. **No claim is established without 3+ independent Tier B+ sources.** Fewer = label accordingly.
4. **Seek disconfirmation for every major claim.** The contrarian search round is not optional.
5. **Don't stop before 3 channels and 8 searches (deep) or 5 searches (quick).**
6. **Verify independently.** Do not re-read the draft while checking claims. Evidence-first.
7. **Classify the unverifiable.** State WHY a claim can't be verified, not just that it can't.
</important>

## Gotchas

1. **Summarizing prematurely.** Gather first, synthesize second. Don't start writing findings after 3 searches.
2. **Citing secondary sources as primary.** When a blog cites a study, fetch the original via Crossref.
3. **Confidence inflation.** Most common error. When in doubt, round DOWN.
4. **Organizing by source instead of by question.** Layer 2 should group findings by sub-question.
5. **Skipping academic search for "practical" topics.** Most practical claims have an evidence base or conspicuously lack one.
6. **Collapsing domain assessment.** "Strong evidence" does not mean "Strong confidence" if Directness is weak. Assess each domain independently.
7. **Performative adversarial review.** If the premortem changed nothing, ask whether the critique was shallow before claiming robustness.
8. **Running full verification every pass.** Early passes get lightweight verification. Full triage runs in the final pass only.

## Success Criteria

1. Every factual claim has a confidence label with domain rationale (or simplified label at Light level)
2. Source log includes all channels used, queries run, and coverage verification results
3. Audit Notes contain premortem results, verification summary, and honest gap disclosure
4. Spiral terminated for a documented reason (triggers resolved, cap reached, or diminishing returns)
5. Output follows the template structure for the selected mode
6. Shelf-life markers have observable signposts, not just time horizons
7. Unverifiable claims are classified by reason, not uniformly flagged
