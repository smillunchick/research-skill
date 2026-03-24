# Research Skill for Claude Code

A prod-level AI research system that replaces "ask Claude and hope for the best" with structured, evidence-grounded research. Convergence-driven spiral process, GRADE-inspired confidence domains, claim verification pipeline, and adversarial review.

Every claim gets a confidence rating. Every source gets a tier. Every conclusion gets stress-tested.

## Install

### Claude Code

```bash
claude install-skill https://github.com/smillunchick/research-skill
```

That's it. The skill is available immediately via `/research` or by asking Claude to research anything.

**Manual install** (if you prefer): copy `SKILL.md` and the `references/` directory to `~/.claude/skills/research/`.

### Claude Code Cowork

Same command from any Cowork session:

```bash
claude install-skill https://github.com/smillunchick/research-skill
```

Or add to your project's `.claude/skills/` directory so it's available to all collaborators:

```bash
mkdir -p .claude/skills/research
cp SKILL.md .claude/skills/research/
cp -r references .claude/skills/research/
```

### Verify installation

After installing, type `/research` in any Claude Code session. You should see the skill activate with mode options. Or just ask:

> "Research the current state of AI-native professional services firms"

If the skill is installed, you'll get structured output with confidence ratings, source tiers, and an adversarial review section.

## What it does

10 research modes, all following the same evidence standards:

| Mode | What you get | When to use |
|------|-------------|-------------|
| `deep` | Full spiral research, 4,000-8,000 words | Default. Serious research on any topic |
| `quick` | Research brief, ~1,500-2,500 words | Time-constrained decisions |
| `sources` | Annotated source list by tier | Building a reading list or bibliography |
| `contrarian` | Steelmanned minority views | Challenging your assumptions |
| `map` | Domain map: researchers, institutions, debates | Orienting in unfamiliar territory |
| `tactics` | Actionable tactics only, evidence-grounded | "What should I actually do?" |
| `update` | Revisit existing research with new evidence | Keeping research current |
| `harvest` | Extract structured data from a URL or document | Processing specific sources |
| `audit` | Re-evaluate existing research for staleness | Quality control |
| `status` | Show current research state | Checking what you've already researched |

### How it works

**Spiral process** — instead of linear "research then write," the system spirals: initial survey, identify gaps, targeted deep-dives, synthesis, verification. Each pass refines the previous one. Convergence (not word count) determines when to stop.

**GRADE confidence** — borrowed from medical research. Every claim rated by evidence quality: high, moderate, low, or very low. You always know which findings you can bet on and which are educated guesses.

**Claim verification** — inspired by CoVe (Chain of Verification). Suspicious claims get decomposed into independently verifiable sub-claims. "AI implementation has a 70% failure rate" becomes: where did this number originate? Who cited it? Does the original source support the claim as stated?

**Adversarial review** — every research output gets a premortem ("how could this analysis be wrong?") and targeted critique. The system surfaces what you'd rather not hear before your stakeholders do.

**Source hierarchy** — five tiers from peer-reviewed research to training-data-only claims. A finding supported by three blog posts citing each other is rated differently than one supported by an independent analyst report.

## Structure

```
SKILL.md                        # The skill (Claude Code reads this)
references/
  source-hierarchy.md           # Source tier definitions and evaluation criteria
  mcp-tools.md                  # MCP tool reference for enhanced source discovery
evals/
  test-inputs.json              # Test scenarios for skill evaluation
  rubric.json                   # Scoring rubric for output quality
```

## Enhanced source discovery (optional)

The skill works with Claude's built-in knowledge, but gets significantly better with MCP research tools. If you have any of these configured, the skill automatically uses them:

- **Exa** — semantic search, company research, people search
- **Firecrawl** — web scraping, site crawling, content extraction
- **Brave Search** — web search, news search
- **Tavily** — web search, research mode

No configuration needed. The skill detects available tools and adapts. Without them, it still produces structured, confidence-rated research — just with a narrower source base.

## Usage examples

```
/research deep "What's the actual evidence for AI improving professional services firm margins?"

/research quick "Current state of the legal AI market"

/research contrarian "The case against AI agents for enterprise"

/research sources "Academic research on human-AI collaboration in consulting"

/research tactics "How to price AI implementation services for mid-market firms"
```

Or just talk naturally:

> "I need to understand the competitive landscape for AI-native accounting firms. Who's doing this, what's working, and what's hype?"

The skill activates automatically when it detects a research intent.

## Want more?

This research skill is one piece of a larger system. I build AI-native skill systems for operators — research, prospect intelligence, content pipelines, signal detection, competitive analysis — each with the same structural guarantees: evidence-grounded, confidence-rated, adversarial-tested.

If you want help building custom skills for your team or extending this system for your specific domain:

**[sam@thinkmetis.co](mailto:sam@thinkmetis.co)** | **[thinkmetis.co](https://thinkmetis.co)**

---

Built by [Sam Millunchick](https://thinkmetis.co) at Metis.
