### Domain 2: Chromatic Depth and Dark Mode Sophistication

For a premium AI tool, dark mode is not a toggle — it is the definitive indicator of a professional, pro-tier environment. It reduces optical strain, optimizes OLED power, and provides a cinematic backdrop where AI-generated visualizations and accent colors achieve striking clarity.

---

#### 2.1 Base Canvas Colors (The Halation Problem)

Pure black (#000000) backgrounds paired with pure white (#FFFFFF) text create excessive contrast that induces **halation** — bright text appears to bleed into dark backgrounds, causing eye fatigue and diminished reading comprehension.

##### Canvas Color Standards

| Surface | HSL Concept | Example Hex | Usage |
|---------|-------------|-------------|-------|
| Base Canvas | hsl(220, 15%, 8%) | `#12141A` | Underlying app background (furthest from user) |
| Surface Level 1 | hsl(220, 15%, 12%) | `#1A1D26` | Bento cards, sidebar backgrounds |
| Surface Level 2 | hsl(220, 15%, 16%) | `#222733` | Interactive hover states, secondary buttons |
| Floating Modal | hsl(220, 15%, 20%) | `#2B3140` | Command palettes, dropdowns, tooltips |
| Primary Text | hsl(220, 10%, 90%) | `#E3E5E8` | Body copy, headers, primary data |
| Secondary Text | hsl(220, 10%, 60%) | `#8F96A3` | Labels, captions, metadata |

##### Audit Checks

| Check | Pass | Fail |
|-------|------|------|
| Background base | HSL lightness 5-10%, subtly tinted | Pure `#000000` or flat untinted gray |
| Primary text | Off-white (`#E0E0E0` to `#F0F0F0` range) | Pure `#FFFFFF` |
| Contrast ratio | Exceeds WCAG 4.5:1 for body text | Below 4.5:1 or above 18:1 (halation risk) |
| Color tinting | Background subtly tinted with brand hue | Neutral gray with no tint (feels flat/generic) |

##### Detection

```bash
# Pure black backgrounds
grep -rn "bg-black\|bg-\[#000\]\|#000000" src/

# Pure white text
grep -rn "text-white\|text-\[#fff\]\|text-\[#FFF\]\|#ffffff\|#FFFFFF" src/

# Check if dark mode uses tinted grays
grep -rn "hsl\|oklch" src/styles/ tailwind.config.*
```

---

#### 2.2 Elevation via Surface Lightness (Z-Axis Hierarchy)

In dark mode, traditional drop shadows lose efficacy because shadows cannot significantly darken a near-black background. Premium dark interfaces communicate depth by **systematically manipulating surface lightness**.

##### The Lightness Elevation Model

```
Z-Axis (closest to user)
  ↑  Floating Modal    — Lightest  (HSL lightness ~20%)
  |  Surface Level 2   — Lighter   (HSL lightness ~16%)
  |  Surface Level 1   — Light     (HSL lightness ~12%)
  ↓  Base Canvas       — Darkest   (HSL lightness ~8%)
Z-Axis (furthest from user)
```

**Rule**: Every increase in elevation MUST correspond to a measurable increase in surface lightness. If a modal and its parent card share the same background color, the elevation hierarchy is broken.

##### Audit Criteria

| Check | Pass | Fail |
|-------|------|------|
| Layered lightness | Each elevation level is 3-5% lighter HSL | Same background for cards and modals |
| Consistent scale | Lightness steps are mathematically uniform | Random lightness jumps (8% → 20% → 14%) |
| Shadow supplement | Shadows used as supplement, not sole depth cue | Only shadows used for elevation (no lightness change) |

##### Detection

```bash
# Extract all background colors and compare
grep -rn "bg-\[" src/ | grep -v "bg-\[url"
grep -rn "background-color\|background:" src/styles/

# Check for elevation tokens in tailwind config
grep -rn "surface\|elevation\|layer" tailwind.config.*
```

---

#### 2.3 Accent Control and the "Monochrome + One" Philosophy

The entire interface is constructed from sophisticated grays and blacks, punctuated by a **single, highly controlled accent color**. This accent is used exclusively to indicate:

- Active AI generation states
- Critical system status changes
- Primary interaction points (buttons, focus rings)
- Progress indicators

##### Desaturation Rules for Dark Mode

Highly saturated colors vibrate unpleasantly against dark backgrounds. Brand colors MUST be dynamically desaturated and lightened for dark themes.

| Light Mode | Dark Mode Adjusted | Method |
|-----------|-------------------|--------|
| `#FF007F` (vivid pink) | `#D96B8A` (muted rose) | Reduce saturation 30%, increase lightness 15% |
| `#0066FF` (electric blue) | `#6B9FD9` (soft blue) | Reduce saturation 25%, increase lightness 20% |
| `#00FF88` (neon green) | `#6BD9A3` (sage) | Reduce saturation 35%, increase lightness 15% |

##### Audit Criteria

| Check | Pass | Fail |
|-------|------|------|
| Accent count | Single accent color family | 3+ competing accent hues |
| Saturation control | Accent colors desaturated for dark bg | Neon/pure hues on dark backgrounds |
| Accent purpose | Reserved for AI activity + primary CTAs | Accent used decoratively everywhere |
| Gray temperature | All grays share same undertone (warm or cool) | Mixed warm/cool grays ("dirty" effect) |

##### Detection

```bash
# Count distinct accent/brand colors in use
grep -rn "brand-\|accent-\|primary-\|secondary-" src/ | grep -v "node_modules"

# Find highly saturated colors
grep -rn "saturate\|hue-rotate" src/

# Check for multiple competing accent hues
grep -rn "bg-blue-\|bg-purple-\|bg-pink-\|bg-green-\|bg-red-" src/
```

---

#### 2.4 Shadow Realism (Multi-Layer, Color-Matched)

A single-layer `box-shadow` invariably appears artificial and cheap. Premium elevation requires multi-layered shadows simulating real-world ambient light physics.

##### Multi-Layer Shadow Formula

```css
/* Premium shadow (3 layers) */
box-shadow:
  0 1px 2px rgba(var(--shadow-color), 0.12),    /* Contact shadow - tight, dark */
  0 4px 8px rgba(var(--shadow-color), 0.08),     /* Near shadow - medium spread */
  0 16px 32px rgba(var(--shadow-color), 0.04);   /* Ambient shadow - wide, faint */
```

##### Shadow Requirements

| Requirement | Standard | Violation |
|-------------|----------|-----------|
| Layer count | 2-3 layers per elevated element | Single `shadow-lg` or `shadow-xl` |
| Color matching | Shadow hue inherits from background | Pure black (`rgba(0,0,0,...)`) shadows |
| Opacity | Low opacity per layer (0.04-0.15) | High opacity creating harsh edges |
| Irregular shapes | `filter: drop-shadow()` for non-rectangular | `box-shadow` on SVGs/transparent elements |

##### Detection

```bash
# Find single-layer shadow usage
grep -rn "shadow-sm\|shadow-md\|shadow-lg\|shadow-xl\|shadow-2xl" src/

# Find custom shadow definitions
grep -rn "box-shadow" src/

# Check tailwind config for custom shadow tokens
grep -A 10 "boxShadow" tailwind.config.*
```

---

#### 2.5 Scoring for Domain 2

| Check | Weight | Pass Criteria |
|-------|--------|---------------|
| No pure black backgrounds | 3 pts | Base canvas uses tinted dark gray (5-10% HSL lightness) |
| No pure white text | 2 pts | Text uses off-white with subtle warmth |
| Elevation via lightness | 4 pts | Each Z-level has measurably lighter surface |
| Monochrome + One accent | 3 pts | Single accent color family, not decorative |
| Desaturated dark accents | 2 pts | Brand colors softened for dark backgrounds |
| Multi-layer shadows | 3 pts | 2-3 layer shadows, color-matched |
| Gray temperature consistency | 1 pt | All grays share same undertone |
| WCAG contrast compliance | 2 pts | All text exceeds 4.5:1 without halation |
| **Total** | **20 pts** | |
