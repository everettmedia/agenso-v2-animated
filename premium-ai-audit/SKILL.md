---
name: premium-ai-audit
description: "Audit and improve app designs against premium AI interface standards ($60+/mo tier). Evaluates structural grids, chromatic depth, typographic authority, agentic UX patterns, and micro-interaction polish. Based on 2026 enterprise AI dashboard research. Use when auditing existing UI for premium feel, reviewing dark mode implementation, evaluating AI state handling, or improving overall design sophistication."
user-invocable: true
---

# Premium AI Interface Design Audit

Evaluate and improve existing app designs against the architectural standards of world-class, $60+/month AI agent dashboards.

This skill operationalizes research on premium AI interface paradigms into a systematic audit framework with actionable remediation guidance.

---

## Step 1: Determine Audit Scope

Before starting, clarify what's being audited:

| Scope | When to Use |
|-------|-------------|
| **Full App** | Comprehensive premium readiness assessment |
| **Single Page** | Evaluate one view (e.g., dashboard, settings) |
| **Component** | Audit a specific component (e.g., command palette, chat UI) |
| **Dark Mode** | Focused chromatic and elevation review |
| **AI States** | Focused review of loading, streaming, agentic UX |

Ask the user which scope applies. Default to **Full App** if unclear.

---

## Step 2: Load Domain Guidelines

Based on scope, load relevant domain files for complete audit criteria:

| Domain | File | Focus Areas |
|--------|------|-------------|
| 1. Structural Grid Integrity | [domains/structural-grid.md](domains/structural-grid.md) | Bento grid, Linear UX, spatial tokens, border radii |
| 2. Chromatic Depth | [domains/chromatic-depth.md](domains/chromatic-depth.md) | Dark mode canvas, elevation via lightness, accent control, shadow realism |
| 3. Typographic Authority | [domains/typographic-authority.md](domains/typographic-authority.md) | Font selection, scaling ratios, line-height rigor, tracking |
| 4. Agentic UX | [domains/agentic-ux.md](domains/agentic-ux.md) | AI thinking states, streaming, command palette, agent transparency |
| 5. Micro-Interaction Polish | [domains/micro-interactions.md](domains/micro-interactions.md) | Tactile feedback, motion restraint, glassmorphism, spatial readiness |

**For Full App audits**: Load all 5 domains.
**For targeted audits**: Load only relevant domains.

---

## Step 3: Execute Audit

### 3.1 Static Code Analysis

Run these checks against the codebase:

```bash
# STRUCTURAL: Find arbitrary spacing values breaking 8px grid
grep -r "\[.*px\]" src/ | grep -v "node_modules"

# STRUCTURAL: Check border-radius consistency
grep -r "rounded-" src/

# CHROMATIC: Find pure black backgrounds
grep -r "bg-black" src/
grep -r "bg-\[#000" src/
grep -r "#000000" src/

# CHROMATIC: Find pure white text on dark
grep -r "text-white" src/

# CHROMATIC: Find hardcoded colors (should be tokens)
grep -r "bg-\[#" src/
grep -r "text-\[#" src/
grep -r "border-\[#" src/

# TYPOGRAPHIC: Check font families in use
grep -r "font-" src/ | grep -v "font-size\|font-weight\|fontawesome"

# TYPOGRAPHIC: Find arbitrary font sizes
grep -r "text-\[" src/

# AGENTIC: Check for loading spinners (should be shimmer)
grep -r "spinner\|loading-circle\|animate-spin" src/

# AGENTIC: Check for skeleton/shimmer patterns
grep -r "shimmer\|skeleton\|animate-pulse" src/

# MICRO: Check for transition durations
grep -r "duration-" src/
grep -r "transition" src/

# MICRO: Check for backdrop-blur usage
grep -r "backdrop-blur\|backdrop-filter" src/

# ELEVATION: Check shadow implementations
grep -r "shadow-" src/
grep -r "box-shadow" src/
```

### 3.2 Visual Inspection (via Playwright MCP)

If Playwright is available:

1. Navigate to each audited page
2. Screenshot at 1440px (desktop target)
3. Screenshot at 375px (mobile)
4. Check browser console for errors
5. Evaluate visual hierarchy, spacing rhythm, color harmony

### 3.3 Cross-Reference with Design Tokens

Read `tailwind.config.mjs` to verify:
- Color tokens follow tinted-gray dark mode scale
- Spacing tokens align with 8px base grid
- Border-radius tokens use concentric nesting scale
- Font family tokens specify neo-grotesque sans-serif + monospace pairing

---

## Step 4: Score Using Premium AI Audit Matrix

### Premium Design Index (PDI)

```
PDI = 100 - (Σ(Severity Weight × Issue Count) / Complexity Factor)

Complexity Factor = 1 + (Component Count / 10)

Severity Weights:
  P1 (Blocker)     = 100 points  — Destroys premium perception
  P2 (Critical)    = 50 points   — Noticeably cheap/amateur
  P3 (Major)       = 20 points   — Suboptimal but tolerable
  P4 (Polish)      = 5 points    — Missed opportunity for excellence

Thresholds:
  Score > 90  → Premium Ready
  Score 75-89 → Approaching Premium (targeted fixes needed)
  Score 60-74 → Standard SaaS (significant uplift required)
  Score < 60  → Below Standard (fundamental redesign needed)
```

---

## Step 5: Generate Report

### Report Structure

```markdown
# Premium AI Interface Audit Report
**Date**: [date]
**Scope**: [Full App / Page / Component]
**PDI Score**: [X]/100 — [Premium Ready / Approaching / Standard / Below]

---

## Executive Summary
[2-3 sentence overview of findings]

## Domain Scores

| Domain | Score | Status |
|--------|-------|--------|
| 1. Structural Grid | X/20 | [Pass/Warn/Fail] |
| 2. Chromatic Depth | X/20 | [Pass/Warn/Fail] |
| 3. Typographic Authority | X/20 | [Pass/Warn/Fail] |
| 4. Agentic UX | X/20 | [Pass/Warn/Fail] |
| 5. Micro-Interaction Polish | X/20 | [Pass/Warn/Fail] |

## Critical Findings (P1-P2)
[List with file paths, line numbers, and remediation]

## Major Findings (P3)
[List with recommendations]

## Polish Opportunities (P4)
[List with suggestions]

## Remediation Priority
[Ordered list of fixes by impact]

## Implementation Checklist
- [ ] Fix 1
- [ ] Fix 2
- ...
```

---

## Step 6: Remediation Mode (Optional)

If the user requests fixes after the audit:

1. **Prioritize P1 blockers first** — fix anything that destroys premium perception
2. **Batch P2 criticals by domain** — chromatic fixes together, structural fixes together
3. **Apply fixes incrementally** — verify each change via Playwright before moving on
4. **Re-audit after fixes** — recalculate PDI to confirm improvement

---

## Quick Reference: The Non-Negotiables

These are the absolute minimum requirements for a premium AI interface:

| Requirement | Standard | Common Violation |
|-------------|----------|------------------|
| No pure black bg | Base canvas HSL lightness 5-10% | Using `bg-black` or `#000000` |
| No pure white text | Off-white `#E0E0E0` to `#F0F0F0` | Using `text-white` on dark bg |
| 8px spatial grid | All spacing divisible by 8 (4 for micro) | Arbitrary values like 13px, 17px |
| Concentric border radii | Inner < Outer (e.g., 8px buttons in 16px cards) | Flat same-radius everywhere |
| No loading spinners | Shimmer/skeleton for AI states | `animate-spin` circle spinner |
| Monochrome + One accent | Neutral grays + single controlled accent | Multiple saturated colors |
| Neo-grotesque sans-serif | Geist, Inter, SF Pro (not decorative) | Using rounded/humanist fonts |
| Monospace for code/data | Matched monospace companion font | Sans-serif for code blocks |
| Multi-layer shadows | 2-3 layers, color-matched, low opacity | Single harsh `shadow-lg` |
| Sub-300ms transitions | Functional, not decorative animation | Slow bouncy entrance animations |
| Desaturated dark accents | Soften brand colors for dark mode | Same neon hex as light mode |
| Elevation via lightness | Lighter surfaces = higher Z | Only using shadows for depth |

---

## Integration Points

This skill works with:

- **`/design-review`**: General design QA (this skill is the premium AI-specific layer)
- **`/visual-validator`**: Screenshot comparison via Playwright
- **`/design-director-master-skill`**: Aesthetic profile validation
- **`/design-elements`**: Implementation of recommended fixes
- **`/spacing`**: Fixing spatial token violations
- **`/font`** / **`/font-sizes`**: Fixing typographic violations
- **`/dark-mode` fixes**: Chromatic depth remediation
