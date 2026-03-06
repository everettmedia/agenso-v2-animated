### Domain 4: Agentic UX — AI State and Interaction Protocols

Traditional web patterns (forms, static dashboards, request-response cycles) are vastly insufficient for managing autonomous AI agent behavior. The UI must communicate intent, reasoning process, and execution progress — not just final state.

---

#### 4.1 Pre-Generation States (The "Thinking" Problem)

Between user prompt submission and the first token arriving (typically 500ms-2s), the interface must communicate deep computational processing without resorting to amateur patterns.

##### Prohibited Patterns

| Pattern | Why It Fails |
|---------|-------------|
| Circular loading spinner (`animate-spin`) | Reads as "broken" or "stuck" — no progress indication |
| Static "Loading..." text | Zero information density, feels unresponsive |
| Empty container with no feedback | User questions if action registered |
| Full-page overlay blocker | Destroys spatial context, feels slow |

##### Required Patterns

**Shimmer UI / Skeleton Screens**

Premium shimmer uses CSS `linear-gradient` animations passing over placeholder shapes, mimicking light flowing across a surface.

For AI text prediction, the skeleton should NOT be a basic rectangle. It should consist of **3-5 lines of varying widths** that mimic the jagged right edge of natural text paragraphs.

```css
/* Premium shimmer example */
.shimmer {
  background: linear-gradient(
    90deg,
    var(--surface-1) 0%,
    var(--surface-2) 50%,
    var(--surface-1) 100%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s ease-in-out infinite;
}
```

**"AI Thinking" Border Glow**

An animated `conic-gradient` rotation on a pseudo-element behind the container, emitting a sophisticated scanning luminescence at the edges. Transforms waiting into technological theater.

```css
/* AI thinking state - rotating border glow */
.ai-thinking::before {
  content: '';
  position: absolute;
  inset: -1px;
  background: conic-gradient(
    from var(--angle),
    transparent 0%,
    var(--accent) 10%,
    transparent 20%
  );
  border-radius: inherit;
  animation: rotate 2s linear infinite;
  z-index: -1;
}
```

##### Audit Criteria

| Check | Pass | Fail |
|-------|------|------|
| Pre-generation feedback | Shimmer/skeleton with text-like shapes | Spinner or static text |
| Shimmer quality | Multi-line varying widths, smooth gradient | Single rectangle placeholder |
| Thinking indicator | Animated border glow or pulsing accent | No visual indication of processing |
| Localized scope | Only affected container shows loading | Full-page loading overlay |

##### Detection

```bash
# Find loading spinners (violations)
grep -rn "animate-spin\|spinner\|loading-circle\|fa-spinner" src/

# Find shimmer/skeleton (good patterns)
grep -rn "shimmer\|skeleton\|animate-pulse\|placeholder" src/

# Check for thinking state implementations
grep -rn "thinking\|generating\|processing" src/
```

---

#### 4.2 Token Streaming Architecture

The frontend MUST render text **token-by-token** as it arrives from the server. This dramatically reduces perceived latency by letting users read the first sentence while the AI computes the third.

##### Streaming Requirements

| Requirement | Standard | Violation |
|-------------|----------|-----------|
| Render mode | Incremental (token-by-token or chunk) | Wait for complete response then render all |
| Layout stability | Text area pre-allocated, no CLS on new tokens | Container resizes violently with each token |
| Cursor/caret | Animated cursor at insertion point | No visual indicator of generation position |
| Scroll behavior | Auto-scroll follows generation, user scroll overrides | Forced scroll that fights user intent |
| Interrupt capability | User can stop generation mid-stream | Must wait for complete generation |

##### Visual Streaming Indicators

| Element | Implementation |
|---------|---------------|
| Typing cursor | Blinking block or line cursor at generation point |
| Progress indicator | Subtle token count or percentage |
| Stop button | Clearly visible, immediately responsive |
| Completion signal | Cursor disappears, subtle completion animation |

##### Detection

```bash
# Check for streaming patterns
grep -rn "stream\|SSE\|EventSource\|ReadableStream\|chunk" src/

# Check for stop/cancel generation
grep -rn "abort\|cancel\|stop.*generat" src/

# Check for auto-scroll behavior
grep -rn "scrollIntoView\|scrollTo\|auto-scroll" src/
```

---

#### 4.3 The Command Palette (Cmd+K)

The Command Palette is the central nervous system of a premium AI application — a searchable, instant-access overlay that hovers above current context. It serves as both navigation AND the primary natural language input.

##### Requirements

| Feature | Standard | Violation |
|---------|----------|-----------|
| Invocation | Universal keyboard shortcut (Cmd+K / Ctrl+K) | No keyboard shortcut, mouse-only access |
| Search scope | Fuzzy search across navigation + commands + prompts | Limited to page navigation only |
| Input types | Natural language ("Analyze Q3 anomaly") AND commands ("/export-pdf") | Only structured commands, no NL |
| Typography | Larger, more legible input field than standard inputs | Same size as regular form inputs |
| Keyboard nav | Full arrow-key navigation with instant highlighting | Mouse-only result selection |
| Visual layer | Glassmorphic overlay maintaining spatial context | Opaque modal destroying context |
| Feedback | Immediate visual confirmation on command execution | No feedback after selection |
| Recency | Recent commands/prompts shown on empty state | Empty state with no suggestions |

##### Detection

```bash
# Check for command palette implementation
grep -rn "command-palette\|cmd-k\|cmdk\|CommandMenu\|CommandPalette" src/

# Check for keyboard shortcut binding
grep -rn "Cmd+K\|Ctrl+K\|metaKey.*k\|ctrlKey.*k" src/

# Check for search/fuzzy matching
grep -rn "fuzzy\|fuse\.js\|search.*command\|filter.*command" src/
```

---

#### 4.4 Agent Transparency (Anti-Black-Box)

When an AI agent calls external tools (web scraper, database query, API call), the UI MUST expose this activity in real-time. The "black box" effect — where the AI works invisibly — creates anxiety and destroys trust.

##### Tool Call Visibility

| State | Visual Treatment |
|-------|-----------------|
| Tool initiated | Compact animated badge: "Searching web..." with activity icon |
| Tool executing | Progress indicator with tool name and target |
| Tool completed | Brief success flash, result summary available |
| Tool failed | Error state with retry option |
| Multiple tools | Stacked badges showing parallel execution |

##### AG-UI Protocol Alignment

The interface should handle these event categories at minimum:

| Event | UI Response |
|-------|-------------|
| TextGeneration | Stream tokens to output area |
| ToolCallStart | Display tool execution badge |
| ToolCallEnd | Show completion, make result inspectable |
| ThinkingStart | Activate thinking state animation |
| StateUpdate | Update relevant dashboard component |
| Error | Display error with context and recovery options |

##### Human Override Requirements

| Control | Required |
|---------|----------|
| Pause/resume agent | User can halt execution at any point |
| Cancel operation | Immediate stop with rollback if applicable |
| Edit mid-stream | User can modify prompt/parameters during execution |
| Approve tool calls | Option to require confirmation before external calls |

##### Detection

```bash
# Check for tool call UI patterns
grep -rn "tool-call\|tool.*badge\|agent.*status\|execution.*status" src/

# Check for human override controls
grep -rn "pause\|resume\|cancel\|stop\|interrupt" src/

# Check for real-time status updates
grep -rn "status.*update\|progress.*update\|event.*stream" src/
```

---

#### 4.5 Scoring for Domain 4

| Check | Weight | Pass Criteria |
|-------|--------|---------------|
| Shimmer/skeleton (no spinners) | 4 pts | Multi-line shimmer for AI text, no circular spinners |
| AI thinking state animation | 2 pts | Border glow or equivalent during pre-generation |
| Token streaming architecture | 4 pts | Incremental render, no layout shift, cursor indicator |
| Command palette present | 3 pts | Cmd+K accessible, fuzzy search, NL + commands |
| Agent tool call transparency | 3 pts | Real-time tool execution badges visible to user |
| Human override controls | 2 pts | Stop/pause/cancel available during AI execution |
| Streaming interrupt capability | 2 pts | User can stop generation mid-stream |
| **Total** | **20 pts** | |
