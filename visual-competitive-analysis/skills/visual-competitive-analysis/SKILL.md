---
name: visual-competitive-analysis
description: Comprehensive visual identity audit for competitive brand research. Use when analyzing competitor brands, researching visual identity systems, or preparing brand strategy documents.
---

# Visual Competitive Analysis

Analyze competitors' full visual identity systems to uncover positioning, design patterns, and strategic opportunities for brand launches or renewals.

## Output Rules
- Tables only — no prose explanations
- Skip unavailable fields entirely
- Max 5 keywords per field unless specified
- No intro or outro sentences
- Deliver section by section, not all at once

## Workflow

### Step 1 — Input
Collect from user:
1. Competitor names (URLs if available)
2. Market / industry context
3. Priority competitors if any

If 5+ competitors → ask: "Analyze in batches of 3 for accuracy?"

### Step 2 — Research
Per competitor, search:
- Official website
- Instagram / TikTok / SNS feeds
- App Store listing (if app exists)
- Packaging: "[brand] packaging design"
- Ad creatives: "[brand] advertising campaign"

### Step 3 — Per-Brand Audit
Run audit framework from: refs/audit-framework.md
Output one brand at a time → confirm before next.

### Step 4 — Comparison Table
After all brands: run refs/comparison-table.md

### Step 5 — Positioning Map
Run refs/positioning-map.md
Auto-propose X/Y axes based on analysis findings.

## Commands
| Command | Action |
|---------|--------|
| /vca [brands] | Full visual competitive analysis |
| /vca-brand [brand] | Single brand deep dive |
| /vca-compare | Comparison table from prior analysis |
| /vca-map | Positioning map only |
