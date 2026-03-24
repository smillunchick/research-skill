# Source Hierarchy & Quality Signals

## Source Tiers

| Tier | Source Type | Examples | Default Trust |
|------|-----------|----------|---------------|
| **S** | Systematic reviews, meta-analyses, RCTs | Cochrane reviews, large-scale replicated studies | High |
| **A** | Peer-reviewed research, longitudinal studies | Journal articles, well-designed observational studies | Moderate-High |
| **B** | Practitioner evidence, authoritative books | Books by domain experts with track records, case studies from credible orgs | Moderate |
| **C** | Expert opinion, conference talks, white papers | Respected researchers' non-peer-reviewed work, industry reports | Moderate-Low |
| **D** | Blog posts, podcasts, social media, journalism | Useful for leads and framing, not for establishing facts | Low (corroboration required) |

## Quality Signals

**Green flags** (increase trust):
- Pre-registered study design
- Large sample size (n > 500 for survey/experimental; n > 50 for clinical)
- Replicated findings across independent labs
- Author has multiple publications in the field
- Published in a high-impact journal for the discipline
- Effect sizes reported alongside p-values
- Open data / open materials

**Red flags** (decrease trust):
- Small sample with large claims
- Single study presented as definitive
- No effect size reported, only "statistically significant"
- Published in a predatory or pay-to-play journal
- Author's only publication on this topic
- Study funded by a party with a financial interest in the outcome
- No control group or comparison condition
- Cherry-picked time periods or subgroups

## Triangulation Rule

No claim is stated as established unless it appears in 3+ independent sources at Tier B or above. Fewer sources = label accordingly.

## Confidence Framework — 4-Domain Assessment

Each claim assessed across 4 independent domains. Do NOT collapse — "strong evidence" with weak directness is NOT the same as "strong confidence."

### Core Domains

| Domain | Measures | Key Question |
|--------|----------|--------------|
| **Evidence strength** | Source tier, sample size, study design, replication | How strong is the underlying evidence? |
| **Consistency** | Agreement across independent sources | Do independent sources agree? |
| **Directness** | Whether evidence addresses the actual question | Does this answer the user's question, or an adjacent one? |
| **Synthesis integrity** | Whether connections between facts are logical | Are the inferences sound, or is this a leap? |

### Optional Flags

| Flag | Apply when |
|------|-----------|
| ⚠ **Precision concern** | Quantitative claims without confidence intervals, small samples, suspiciously round numbers |
| ⚠ **Completeness concern** | Suspected missing studies, file-drawer problem, search only in English |

### Overall Labels

| Label | Domain Profile | Inline Format |
|-------|---------------|---------------|
| **Strong** | All 4 domains solid, no flags | `[Strong — direct, consistent, multi-source]` |
| **Moderate** | 1 domain weak or 1 flag active | `[Moderate — strong evidence, indirect]` |
| **Emerging** | 2+ domains weak, limited evidence | `[Emerging — single source, direct, logical]` |
| **Speculative** | Thin evidence, extrapolation, unverified | `[Speculative — extrapolation from adjacent field]` |

### Calibration Rules

- **Reference class anchoring:** Consider the outside view — what's the base rate for claims like this?
- **Internal consistency:** Same evidence type → same confidence across claims
- **When in doubt, round DOWN** — overconfidence is the most common error
- Can't find original source → Emerging ceiling regardless of plausibility
- Two credible sources contradict → Moderate ceiling for the contested claim
- Training data claims unverified via search → Emerging ceiling + `[From training data — not verified]`

### Simplified Labels (Light Verification)

At Light verification level, use single-dimension labels without domain breakdown:
- **Strong** — multiple high-quality sources, directly relevant
- **Moderate** — some evidence, minor caveats
- **Emerging** — limited evidence, promising but early
- **Speculative** — extrapolation or unverified
