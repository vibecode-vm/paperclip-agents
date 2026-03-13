# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.

---

## Skills

### 1. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

When browser automation fails:
1. **Root Cause**: Screenshot the state. Is the element there? Is it visible? Is it covered by an overlay? Is the page still loading?
2. **Pattern Analysis**: Did the same command work before? What changed? (DOM, timing, viewport, auth state)
3. **Hypothesis Testing**: ONE fix — add `wait`? Fix selector? Scroll into view? Close modal first?
4. **Implementation**: Fix at root cause. NEVER use `sleep()` as a fix — use condition-based waiting.

**Defensive Automation Patterns:**
- Always `wait --load networkidle` before interacting
- Use `@element-id` selectors over text selectors (more stable)
- Screenshot BEFORE and AFTER each critical action for debugging
- If element not found: `snapshot -i` first → verify selector exists

### 2. Verification — verification-before-completion (obra/superpowers, 16K)
Screenshot evidence for every claim. "I checked the page" is not evidence — a screenshot is.

---

## Primary: agent-browser (vercel-labs, 89K)

Full API reference for browser automation.

### Session Management
```bash
# Open a page
agent-browser open <url>
agent-browser open <url> --incognito

# Wait for page load
agent-browser wait --load networkidle    # All network requests settled
agent-browser wait --load domcontentloaded  # DOM loaded
agent-browser wait --text "Expected"     # Wait for text to appear
agent-browser wait --selector @element   # Wait for element

# Close session
agent-browser close
agent-browser close --all
```

### Viewport & Display
```bash
# Set viewport size
agent-browser set viewport <width> <height>

# Viewport presets
agent-browser set viewport 375 812    # iPhone SE / Mobile
agent-browser set viewport 390 844    # iPhone 14
agent-browser set viewport 768 1024   # iPad / Tablet
agent-browser set viewport 1024 768   # iPad Landscape
agent-browser set viewport 1440 900   # Desktop
agent-browser set viewport 1920 1080  # Full HD

# Color scheme
agent-browser --color-scheme dark open <url>
agent-browser --color-scheme light open <url>
```

### DOM Interaction
```bash
# Click
agent-browser click @element-id
agent-browser click "Button Text"

# Type
agent-browser type @input-id "text value"
agent-browser type @input-id ""  # Clear input

# Select
agent-browser select @select-id "option-value"

# Scroll
agent-browser scroll down 500
agent-browser scroll up 300
agent-browser scroll @element-id

# Focus
agent-browser focus @element-id
agent-browser tab  # Tab to next focusable
```

### Information Retrieval
```bash
# Read text
agent-browser get text @element-id
agent-browser get value @input-id
agent-browser get attribute @element-id "href"

# DOM snapshot
agent-browser snapshot          # Full page structure
agent-browser snapshot -i       # Interactive elements only
agent-browser snapshot -i -C    # Interactive + focus order

# Page info
agent-browser get url
agent-browser get title
agent-browser get cookies
```

### Capture
```bash
# Screenshot
agent-browser screenshot filename.png
agent-browser screenshot --full-page full.png
agent-browser screenshot --element @component component.png

# Video recording
agent-browser record start session.mp4
agent-browser record stop
agent-browser record pause
agent-browser record resume

# Performance
agent-browser profiler start
# ... actions ...
agent-browser profiler stop
agent-browser profiler report
```

### Network Monitoring
```bash
# Monitor requests
agent-browser network log start
agent-browser network log stop

# Check console
agent-browser console errors     # Show JS errors
agent-browser console warnings   # Show warnings
agent-browser console log        # Show all console output
```

### Highlight & Debug
```bash
# Visual debugging
agent-browser highlight @element-id
agent-browser highlight @element-id --color red
agent-browser highlight --remove

# Accessibility
agent-browser a11y audit         # Run accessibility audit
agent-browser a11y tree          # Show accessibility tree
```

---

## Browser Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| Headless | Yes | Run without visible window |
| Timeout | 30s | Default navigation timeout |
| User Agent | Chrome latest | Browser identity |
| JavaScript | Enabled | JS execution |
| Images | Enabled | Image loading |
| Cookies | Session | Cookie persistence |

---

## Error Handling

| Error | Cause | Resolution |
|-------|-------|------------|
| Navigation timeout | Page too slow | Increase timeout or check URL |
| Element not found | Wrong selector | Check element ID, use snapshot |
| Click intercepted | Element covered | Scroll or close overlay |
| Session crashed | Browser crash | Restart session |
| Network error | No connection | Check URL and network |
