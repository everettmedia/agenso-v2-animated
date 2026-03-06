### Domain 1: Structural Grid Integrity

The foundation of any premium AI interface is its structural integrity. The layout must manage complex, multi-modal data streams without overwhelming cognitive capacity while projecting an aura of control.

---

#### 1.1 Bento Grid Architecture

The Bento Grid is the definitive structural standard for high-end AI dashboard architecture. Content is restricted into modular, self-contained sections of varying sizes (1x1, 2x1, 2x2 blocks) within a strict underlying geometric grid.

##### Why Bento for AI

- **Disparate data isolation**: Chat interfaces, real-time visualizations, execution statuses, and generated artifacts coexist without visual friction
- **Component modularity**: Each block functions as an independent stateful rendering unit with its own loading mechanism
- **Localized state**: When an AI agent is "thinking," only the relevant block enters a loading state while the rest remains interactive
- **Reduced perceived latency**: Localized rendering prevents full-dashboard blocking

##### Audit Criteria

| Attribute | Standard | Violation |
|-----------|----------|-----------|
| Hierarchy via Scale | 2x2 blocks for primary outputs, 1x1 for secondary metrics | All blocks same size (no visual hierarchy) |
| Column Limit | Max 3-4 columns on desktop viewports | 5+ columns causing cognitive overload |
| Edge Uniformity | Consistent border-radius across all blocks | Mixed radii breaking visual cohesion |
| Content Isolation | Each data type in its own bounded container | Mixed content types bleeding across boundaries |
| Independent Loading | Per-block shimmer/skeleton states | Single global loading state for entire dashboard |

##### Detection

```bash
# Check for grid systems
grep -r "grid-cols-" src/
grep -r "grid-template" src/

# Check for excessive columns
grep -r "grid-cols-[5-9]\|grid-cols-1[0-2]" src/

# Check border-radius consistency
grep -r "rounded-" src/ | sort | uniq -c | sort -rn
```

---

#### 1.2 Linear UX Philosophy

"Linear UX" prioritizes sequential, goal-oriented journeys with extreme reduction in visual noise. Software should do the heavy lifting, keeping users productive rather than forcing them to navigate complex nested menus.

##### Core Principles

- **One-dimensional scrolling**: Strict vertical flow, no zig-zagging layouts
- **Absolute text alignment**: All content flows along consistent alignment axes
- **Minimal contextual CTAs**: Only the most relevant forward path based on current AI state
- **Constraints breed speed**: Highly linear paths enable muscle memory and flow states

##### Audit Criteria

| Check | Pass | Fail |
|-------|------|------|
| Scroll direction | Single-axis (vertical) primary flow | Horizontal + vertical mixed scrolling |
| CTA density | 1-2 primary actions visible per viewport | 3+ competing CTAs creating decision fatigue |
| Navigation depth | Max 2 levels for any workflow | Deep nested menus requiring 3+ clicks |
| Alignment axes | Content snaps to 1-2 vertical alignment lines | Multiple misaligned content blocks |
| Proprietary terminology | Standard, recognizable labels | Invented jargon requiring learning curve |

##### Detection

```bash
# Check for horizontal scroll containers (acceptable in carousels, not main content)
grep -r "overflow-x-auto\|overflow-x-scroll" src/

# Count CTAs per component
grep -r "btn\|button\|cta" src/ --include="*.astro"

# Check for deep nesting
grep -r "dropdown\|submenu\|nested-menu" src/
```

---

#### 1.3 Spatial Tokenization (8px Base Grid)

Absolute precision in spacing is the subconscious hallmark of a premium product. The interface must be built on a strict spatial token system using an **8px base grid** with **4px half-steps** reserved exclusively for high-density micro-configurations.

##### Spatial Token Scale

| Token | Pixel Value | Application |
|-------|-------------|-------------|
| `--spacing-2` | 2px | Micro-adjustments, icon-to-text alignment in dense badges |
| `--spacing-4` | 4px | Internal component padding, tight data rows, text-to-subtext gaps |
| `--spacing-8` | 8px | Standard baseline for component gaps and text margins |
| `--spacing-16` | 16px | Container padding, margins between layout blocks |
| `--spacing-24` | 24px | Section dividers, breathing room around primary CTAs |
| `--spacing-32` | 32px | Major layout separations, primary gutters between Bento modules |
| `--spacing-48` | 48px | Large section separations, hero spacing |
| `--spacing-64` | 64px | Major page section boundaries |

##### Violation Detection

**Any spacing value not divisible by 4 is a violation.** Common offenders:

- `p-[13px]`, `m-[17px]`, `gap-[21px]` — arbitrary values breaking rhythm
- `p-3` (12px) — acceptable (divisible by 4)
- `p-5` (20px) — acceptable (divisible by 4)
- `p-[11px]` — violation

##### Border Radius Harmony (Concentric Nesting)

Border radii must follow a parallel mathematical scale creating a concentric nesting effect:

| Context | Radius | Tailwind |
|---------|--------|----------|
| Inner interactive elements (buttons, inputs) | 6-8px | `rounded-md` to `rounded-lg` |
| Outer cards/containers (Bento blocks) | 12-16px | `rounded-xl` to `rounded-2xl` |
| Modal/overlay containers | 16-24px | `rounded-2xl` to `rounded-3xl` |

**Rule**: Inner radius must always be smaller than the outer container radius. This creates aesthetically pleasing concentric curves that reinforce meticulous craftsmanship.

##### Detection

```bash
# Find ALL arbitrary pixel values
grep -rn "\[.*px\]" src/ | grep -v "node_modules"

# Check specific violations (not divisible by 4)
grep -rn "\[1px\]\|\[3px\]\|\[5px\]\|\[7px\]\|\[9px\]\|\[11px\]\|\[13px\]\|\[15px\]\|\[17px\]\|\[19px\]\|\[21px\]" src/

# Audit border-radius distribution
grep -rn "rounded-" src/ | sed 's/.*\(rounded-[a-z0-9]*\).*/\1/' | sort | uniq -c | sort -rn
```

---

#### 1.4 Scoring for Domain 1

| Check | Weight | Pass Criteria |
|-------|--------|---------------|
| Bento/grid structure present | 4 pts | Modular grid with hierarchy via scale |
| Column discipline (max 3-4) | 2 pts | No viewport exceeds 4 primary columns |
| Linear flow (no zig-zag) | 3 pts | Single-axis primary scroll, minimal CTAs |
| All spacing on 8px grid | 4 pts | Zero arbitrary values outside 4px multiples |
| Concentric border radii | 3 pts | Inner < Outer consistently |
| Content isolation per block | 2 pts | Each data type bounded independently |
| Independent loading states | 2 pts | Per-component shimmer, not global spinner |
| **Total** | **20 pts** | |
