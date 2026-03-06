### Domain 3: Typographic Authority

Typography in a premium AI interface serves a dual purpose: flawless legibility at small sizes for dense data, and sleek technical authority that justifies the premium price point. The 2026 standard heavily favors modern geometric or neo-grotesque sans-serifs rooted in Swiss design principles.

---

#### 3.1 Typeface Selection

##### Primary Sans-Serif Requirements

The primary typeface must be a **developer-centric neo-grotesque sans-serif** engineered specifically for digital product environments:

| Tier | Typefaces | Characteristics |
|------|-----------|-----------------|
| **Gold Standard** | Geist Sans, Inter, SF Pro | Excellent x-heights, open apertures, neutral letterforms |
| **Acceptable** | Satoshi, Plus Jakarta Sans, DM Sans | Modern geometric sans with good screen rendering |
| **Avoid** | Rounded fonts (Nunito, Quicksand), humanist fonts (Open Sans), serif fonts | Too soft/friendly for professional AI context |

##### Monospace Companion (Mandatory)

AI tools invariably require a monospace companion for code snippets, JSON outputs, execution logs, and data tables.

| Requirement | Standard | Violation |
|-------------|----------|-----------|
| Monospace presence | Dedicated monospace in design system | No monospace defined, using browser default |
| Metric alignment | Monospace shares x-height/metrics with primary sans | Mismatched proportions creating jarring switches |
| Usage scope | Code blocks, data outputs, terminal logs | Sans-serif used to display code/data |

**Ideal Pairings:**
- Geist Sans + Geist Mono
- Inter + JetBrains Mono
- SF Pro + SF Mono

##### Detection

```bash
# Check font families in tailwind config
grep -A 5 "fontFamily" tailwind.config.*

# Check font imports
grep -rn "@font-face\|font-family\|googleapis.com/css" src/

# Check for monospace usage
grep -rn "font-mono\|fontFamily.*mono\|code\|pre" src/
```

---

#### 3.2 Typographic Scaling (Major Second Ratio)

Font sizes must adhere to a strict modular mathematical scale rather than arbitrary visual estimates. The **Major Second ratio (1.125)** is highly effective for data-dense AI dashboards.

##### Recommended Scale (Base: 14px)

| Step | Size | Tailwind | Usage |
|------|------|----------|-------|
| -2 | 11px | `text-[11px]` or custom | Micro labels, timestamps |
| -1 | 12px | `text-xs` | Captions, badges, metadata |
| 0 (base) | 14px | `text-sm` | Body text, descriptions |
| +1 | 16px | `text-base` | Emphasized body, subheadings |
| +2 | 18px | `text-lg` | Section subheadings |
| +3 | 20px | `text-xl` | Card titles, widget headers |
| +4 | 24px | `text-2xl` | Page section headers |
| +5 | 28px-30px | `text-3xl` | Major section titles |
| +6 | 36px | `text-4xl` | Hero/display headlines |

##### Audit Criteria

| Check | Pass | Fail |
|-------|------|------|
| Scale adherence | All sizes map to defined scale | Arbitrary sizes (17px, 19px, 23px, 27px) |
| Hierarchy descends | Visual weight strictly descends (H1 > H2 > H3) | H3 styled larger than H2 |
| Base size for data | 14px minimum for dense data | 12px body text causing readability issues |
| Consistency | Same semantic level uses same size everywhere | Same heading level renders differently across pages |

##### Detection

```bash
# Extract all text size classes
grep -rn "text-\[" src/ | grep -v "text-\[#\|text-\[rgb\|text-\[hsl"

# Check for arbitrary sizes
grep -rn "font-size:" src/styles/
```

---

#### 3.3 Line-Height Rigor

Line height (leading) is equally critical to quality perception. WCAG 1.4.12 mandates minimum 1.5x for body text.

##### Line-Height Standards

| Text Type | Line Height | Tailwind | Purpose |
|-----------|-------------|----------|---------|
| Body text (14-16px) | 1.5-1.6 | `leading-normal` to `leading-relaxed` | Optimal reading comprehension |
| Data tables | 1.4-1.5 | `leading-snug` to `leading-normal` | Dense but readable rows |
| Large headings (24px+) | 1.1-1.2 | `leading-tight` or `leading-none` | Premium "lockup" feel — tight, anchored |
| Display/hero text (36px+) | 1.0-1.15 | `leading-none` to `leading-tight` | Maximum visual impact |

##### The Premium Lockup Effect

For large headings and display text, **tightened leading + reduced tracking** creates a dense, premium typographic "lockup" that anchors the interface visually. This is a signature characteristic of high-end software.

```
✅ Premium lockup:
   leading-tight + tracking-tight on 36px+ text
   Creates: Dense, authoritative, confident feel

❌ Amateur spacing:
   leading-relaxed + tracking-normal on 36px text
   Creates: Floating, disconnected, unanchored feel
```

##### Tracking Rules

| Text Type | Tracking | Tailwind |
|-----------|----------|----------|
| Large headings (32px+) | Negative (tighter) | `tracking-tight` or `tracking-tighter` |
| Uppercase labels/badges | Positive (wider) | `tracking-wide` or `tracking-widest` |
| Body text | Normal | `tracking-normal` |
| Monospace/code | Normal to slightly wide | `tracking-normal` to `tracking-wide` |

##### Audit Criteria

| Check | Pass | Fail |
|-------|------|------|
| Body line-height | 1.5x minimum (150%) | Below 1.4x causing cramped reading |
| Heading line-height | 1.1-1.2x for 24px+ text | 1.5x+ on large headings (floaty) |
| Large heading tracking | Negative tracking on 32px+ | Default tracking on display text |
| Uppercase tracking | Positive tracking | Default/tight tracking on ALL CAPS |

##### Detection

```bash
# Check line-height usage
grep -rn "leading-" src/

# Check tracking usage
grep -rn "tracking-" src/

# Find large headings without tight leading
grep -rn "text-3xl\|text-4xl\|text-5xl\|text-6xl" src/
# Then verify these have leading-tight or leading-none
```

---

#### 3.4 Font Weight Distribution

Premium interfaces use font weight deliberately to establish hierarchy, not decoratively.

| Weight | Usage | Overuse Signal |
|--------|-------|----------------|
| 400 (Regular) | Body text, descriptions | — |
| 500 (Medium) | Emphasized body, secondary labels | Using for everything |
| 600 (Semibold) | Subheadings, card titles, nav items | — |
| 700 (Bold) | Section headers, primary CTAs | Using on body text |
| 800-900 (Black) | Hero display text only | Using below 36px |

**Rule**: Never use more than 3 weights on a single page/view. Weight proliferation dilutes hierarchy.

---

#### 3.5 Scoring for Domain 3

| Check | Weight | Pass Criteria |
|-------|--------|---------------|
| Neo-grotesque sans-serif primary | 3 pts | Geist/Inter/SF Pro or equivalent |
| Monospace companion present | 2 pts | Dedicated monospace with metric alignment |
| Modular scale adherence | 4 pts | All sizes on mathematical scale, no arbitrary values |
| Heading hierarchy descends | 2 pts | H1 > H2 > H3 visually |
| Body line-height ≥ 1.5x | 2 pts | WCAG compliant leading |
| Heading lockup (tight leading + tracking) | 3 pts | Large text has premium lockup |
| Weight discipline (max 3 per view) | 2 pts | Deliberate weight hierarchy |
| Uppercase tracking applied | 2 pts | Wide tracking on ALL CAPS text |
| **Total** | **20 pts** | |
