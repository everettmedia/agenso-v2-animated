### Domain 5: Micro-Interaction Polish and Kinesthetic Feedback

The fundamental difference between a functional interface and a $60+/month premium experience resides entirely in micro-interactions — single-purpose, highly functional animations providing immediate kinesthetic feedback. The critical distinction: **micro-interactions are functional and rapid; animations are narrative and decorative.** Premium environments strictly avoid the latter.

---

#### 5.1 Tactile Button Responses

When a user clicks a primary action button, the interface must respond with immediate, hardware-like physical feedback mimicking the sensation of depressing a mechanical switch.

##### The Scale-Down Pattern

```css
/* Premium tactile response */
button:active {
  transform: scale(0.97);
  transition: transform 100ms cubic-bezier(0.2, 0, 0, 1);
}
```

##### Requirements

| Element | Expected Response | Duration | Violation |
|---------|-------------------|----------|-----------|
| Primary buttons | Scale(0.97) + slight darken | 80-120ms | No active state, or only color change |
| Secondary buttons | Scale(0.98) or opacity shift | 80-120ms | No feedback at all |
| Toggle switches | Immediate snap with momentum | 150ms | Slow slide animation |
| Card clicks | Subtle scale(0.99) or shadow reduction | 100ms | No click feedback |
| Icon buttons | Scale(0.9) on the icon specifically | 80ms | No response |

##### Trigger-Rule-Feedback-Loop Framework

1. **Trigger**: User's click/tap
2. **Rule**: System determines the action
3. **Feedback**: Visual compression (the tactile scale-down)
4. **Loop**: Return to default state via spring easing

##### Detection

```bash
# Check for active states on buttons
grep -rn "active:" src/ | grep -i "button\|btn\|cta"

# Check for transform scale on interaction
grep -rn "active:scale\|active:transform" src/

# Check for transition timing
grep -rn "duration-\[.*ms\]\|duration-75\|duration-100\|duration-150" src/
```

---

#### 5.2 Motion Restraint (The Anti-Animation Rule)

Premium SaaS environments strictly avoid slow, dramatic animations. While impressive in marketing demos, these interrupt workflows, introduce artificial latency, and cause severe interaction fatigue during daily repetitive use.

##### The 300ms Rule

**All functional transitions must complete in under 300ms.** Any animation exceeding this is decorative and must be justified.

| Interaction | Max Duration | Easing |
|-------------|-------------|--------|
| Hover state changes | 150-200ms | `ease-out` |
| Button feedback | 80-120ms | `cubic-bezier(0.2, 0, 0, 1)` |
| Tooltip appear | 100-200ms | `ease-out` |
| Modal open | 200-300ms | `ease-out` |
| Modal close | 150-250ms | `ease-in` |
| Dropdown expand | 150-250ms | `ease-out` |
| Tab switch | 100-200ms | `ease-out` |
| Content fade-in | 200-300ms | `ease-out` |

##### Prohibited Patterns

| Pattern | Problem | Severity |
|---------|---------|----------|
| Cards flying in from screen edges | Interrupts workflow, feels like a demo | P2 |
| Bouncy spring animations (excessive overshoot) | Childish feel, causes motion sickness | P2 |
| Staggered card reveals > 500ms total | Artificially inflates perceived load time | P3 |
| `ease-linear` on UI movement | Looks robotic and unnatural | P3 |
| Parallax scroll effects in dashboard | Distracting in a tool context | P3 |
| Auto-playing decorative animations | Drains battery, distracts from content | P2 |

##### Easing Requirements

| Direction | Required Easing | Why |
|-----------|-----------------|-----|
| Element entering view | `ease-out` | Feels responsive — fast start, gentle landing |
| Element leaving view | `ease-in` | Feels deliberate — gentle start, fast exit |
| Continuous/looping | `ease-in-out` or `linear` | Smooth sustained motion |
| UI movement (never) | `ease-linear` | Looks mechanical — flag as violation |

##### Detection

```bash
# Find slow animations (violations)
grep -rn "duration-500\|duration-700\|duration-1000\|duration-\[.*[5-9]00\]" src/

# Find bouncy/spring animations
grep -rn "bounce\|spring\|overshoot\|elastic" src/

# Check easing functions
grep -rn "ease-linear" src/

# Find staggered delays
grep -rn "delay-\|animation-delay\|stagger" src/
```

---

#### 5.3 Transition Smoothing and Spatial Preservation

When content updates asynchronously (AI agent completing a task, new data injecting), surrounding elements must slide smoothly to accommodate — never snap violently into place.

##### Layout Shift Prevention

| Scenario | Required Behavior | Violation |
|----------|-------------------|-----------|
| New AI response added | Container smoothly expands with `height: auto` transition | Content pops in, pushing everything down instantly |
| Data visualization loads | Placeholder reserves exact space, content fades in | CLS as chart renders at different dimensions |
| Sidebar collapse/expand | Smooth width transition, content reflows gracefully | Instant toggle causing layout jump |
| Notification appears | Slides in from edge, pushes nothing else | Pushes header/content down |

##### Progressive Feedback for Long Operations

Instead of static "Loading..." text, display:

1. **Smooth progress bars** that fill incrementally
2. **Dynamic micro-task text**: "Analyzing dataset...", "Optimizing parameters...", "Rendering output..."
3. **Step indicators** showing position in multi-step process
4. **Percentage or token count** for quantifiable progress

##### Detection

```bash
# Check for height/layout transitions
grep -rn "transition.*height\|transition.*max-height\|transition.*width" src/

# Check for CLS prevention patterns
grep -rn "min-h-\|min-w-\|aspect-ratio\|skeleton" src/

# Check for progress indicators
grep -rn "progress\|step.*indicator\|percentage" src/
```

---

#### 5.4 Liquid Glass / Glassmorphism (Extreme Restraint)

Glassmorphism ("Liquid Glass") uses `backdrop-filter: blur()` and translucency to create frosted glass effects. In premium environments, this is NEVER decorative — it is a functional semantic layer for temporary elevation.

##### Usage Rules

| Rule | Standard | Violation |
|------|----------|-----------|
| Application scope | ONLY the highest-priority floating UI layer | Multiple blurred layers stacked |
| Fill opacity | Extremely low (0.05-0.10) | Heavy color fill masking content |
| Blur value | `backdrop-blur-lg` to `backdrop-blur-xl` (16-24px) | Light blur that doesn't sufficiently separate |
| Light-catching border | `inset 1px 1px 0px rgba(255,255,255,0.1)` inner edge | No border detail (flat glass) |
| Performance | Single blur layer only | Multiple nested blurs degrading GPU |
| Legibility | All text on glass layer exceeds WCAG contrast | Text becomes unreadable over busy backgrounds |

##### Acceptable Uses

- Command Palette overlay
- AI reasoning/thinking modal
- Contextual tooltip or popover
- Mobile navigation overlay

##### Unacceptable Uses

- Dashboard card backgrounds (use solid elevated surfaces)
- Sidebar navigation (use solid surface colors)
- Multiple stacked glass layers
- Decorative section backgrounds

##### The Light-Catching Border (Critical Detail)

The detail that transforms basic blur into premium glass:

```css
/* The finishing touch */
.glass-element {
  background: rgba(var(--surface-1-rgb), 0.06);
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: inset 1px 1px 0px rgba(255, 255, 255, 0.1);
}
```

##### Detection

```bash
# Find all glassmorphism usage
grep -rn "backdrop-blur\|backdrop-filter\|glass" src/

# Count instances (should be very few)
grep -rn "backdrop-blur" src/ | wc -l

# Check for light-catching border detail
grep -rn "inset.*rgba(255" src/
```

---

#### 5.5 Spatial Computing Readiness

Premium 2D interfaces should be structurally prepared for spatial computing translation (Apple Vision Pro, mixed reality).

##### Structural Preparation

| Requirement | Implementation |
|-------------|---------------|
| Logical depth planes | CSS layers map to distinct Z-levels |
| Touch target sizing | Interactive elements use `--spacing-48` padding minimum |
| Hover state exaggeration | Hover effects are smooth and prominent (for eye-tracking) |
| Voice input readiness | UI can display audio waveform/orb feedback |
| Large hit areas | All interactive elements ≥ 44x44px |

##### Detection

```bash
# Check touch target sizes
grep -rn "p-[0-3] \|p-1\b\|p-2\b\|p-3\b" src/ | grep -i "button\|link\|click\|tap"

# Check for voice/audio UI patterns
grep -rn "voice\|audio\|waveform\|microphone\|speech" src/
```

---

#### 5.6 Scoring for Domain 5

| Check | Weight | Pass Criteria |
|-------|--------|---------------|
| Tactile button responses | 3 pts | Active state scale-down on all interactive elements |
| Sub-300ms transitions | 3 pts | All functional transitions under 300ms |
| No decorative animations | 2 pts | No bouncy/flying/slow narrative animations |
| Correct easing functions | 2 pts | ease-out for enter, ease-in for exit, no linear on UI |
| Smooth layout transitions | 3 pts | No layout snapping, CLS prevention for async content |
| Progressive AI feedback | 2 pts | Dynamic text + progress for long operations |
| Glassmorphism restraint | 3 pts | Max 1 glass layer, only on highest-priority modal |
| Light-catching border detail | 1 pt | Inner white border on glass elements |
| Touch targets ≥ 44px | 1 pt | All interactive elements meet minimum |
| **Total** | **20 pts** | |
