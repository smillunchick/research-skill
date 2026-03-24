# Research Skill for Claude Code

A prod-level AI research system. Convergence-driven spiral process, GRADE-inspired confidence domains, claim verification pipeline, and adversarial review.

## Install

```bash
claude install-skill https://github.com/smillunchick/research-skill
```

Or manually: copy `SKILL.md` and `references/` to `~/.claude/skills/research/`.

## What it does

10 research modes from quick briefs to full deep dives:

- **Spiral process** — converges on truth through iterative refinement, not linear phases
- **GRADE confidence** — every claim rated by evidence quality (high/moderate/low/very low)
- **Claim verification** — evidence-first pipeline inspired by CoVe (Chain of Verification)
- **Adversarial review** — premortem + targeted critique surfaces what you'd rather not hear
- **Source hierarchy** — tiered system distinguishing peer-reviewed from blog posts from training data

## Structure

```
SKILL.md              # The skill definition (Claude Code reads this)
references/
  source-hierarchy.md # Source tier definitions and evaluation criteria
  mcp-tools.md        # MCP tool reference for research sources
evals/
  test-inputs.json    # Test scenarios for skill evaluation
  rubric.json         # Scoring rubric for output quality
```

## Requirements

- [Claude Code](https://claude.com/claude-code) or Cowork
- Optional: Exa, Firecrawl, Brave Search, or Tavily MCP servers for enhanced source discovery (the skill degrades gracefully without them)

## Author

Built by [Sam Millunchick](https://thinkmetis.co) at Metis.
