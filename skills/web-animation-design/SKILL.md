---
name: web-animation-design
description: Design rules for web page animations that simulate app workflows (e.g. Linear, Jira). Use when building or updating animated demos, walkthroughs, or screen-capture-style HTML animations. Covers zoom behavior, dialog sizing, controls bar, playback, and visual style.
---

# Web Animation Design

## Zoom Behavior

- **Zoom in** only when a dialog window or issue/detail panel **opens**. Use `scaleToFit()` so the element fills ~85% of the zoom-layer viewport.
- **Zoom out** (back to `scale(1)`) only when the user **returns to the main window** (dialog/panel closes).
- **No other zoom-in or zoom-out** at any other point in the animation — no zooming into individual text fields, comments, or body text.
- The **controls bar**, **caption overlay**, and **cursor** live outside the zoom layer and are never scaled.

```js
function scaleToFit(rect, padding = 0.85) {
  const zl = document.getElementById('zoom-layer').getBoundingClientRect();
  const sx = (zl.width  * padding) / rect.width;
  const sy = (zl.height * padding) / rect.height;
  return Math.min(sx, sy, 3.5);
}
// Zoom in when dialog opens:
await zoomToEl(dialogEl, scaleToFit(dialogEl.getBoundingClientRect()));
// Zoom out when dialog closes:
await zoomReset();
```

## Dialog & Panel Sizing

- New Issue dialog: `width: 80vw; max-height: 80vh`
- Issue detail panel: `width: 80vw; height: 80vh`

## Scroll Behavior

- When a new comment or the typing indicator appears, **only scroll if the element is not fully visible** in the panel.
- When scrolling, scroll **minimally** — just enough so the bottom of the element aligns with the bottom of the visible area. **Never center** the element.
- If the element is already fully in view, do not scroll at all.

```js
function scrollCommentIntoView(commentEl, containerEl) {
  const cRect = commentEl.getBoundingClientRect();
  const pRect = containerEl.getBoundingClientRect();
  const fullyVisible = cRect.top >= pRect.top && cRect.bottom <= pRect.bottom;
  if (fullyVisible) return;
  let targetScrollTop = containerEl.scrollTop;
  if (cRect.bottom > pRect.bottom) {
    targetScrollTop += cRect.bottom - pRect.bottom; // scroll down minimally
  } else if (cRect.top < pRect.top) {
    targetScrollTop -= pRect.top - cRect.top;       // scroll up minimally
  }
  containerEl.scrollTo({ top: Math.max(0, targetScrollTop), behavior: 'smooth' });
}
```

## Typing Indicator Placement

- The "ML Agent is typing…" indicator must always appear **immediately below the last visible comment**.
- Before showing it, move the indicator node in the DOM to sit directly after the last `.comment-block.visible` element. If no comment is visible yet, prepend it to the comments section.

```js
function showTypingIndicator() {
  const section = document.getElementById('comments-section');
  const indicator = document.getElementById('typing-indicator');
  const comments = Array.from(section.querySelectorAll('.comment-block.visible'));
  if (comments.length > 0) {
    comments[comments.length - 1].after(indicator);
  } else {
    section.prepend(indicator);
  }
  indicator.classList.add('show');
}
```

## Playback Controls (fixed bottom bar, outside zoom layer)

Two rows:

**Row 1 — Step Indicator**
- Step badge: `Step X / N` (indigo pill)
- Step description: current step label, 14px medium weight

**Row 2 — Playback Row**
- ↺ Startover · ⏮ Prev step · ▶/⏸ Play/Pause · ⏭ Next step · [progress bar] · timestamp · speed buttons (0.5× 1× 1.5× 2×)
- Prev/Next step and ← / → arrow keys: jump to step boundary and **auto-pause**
- Space: toggle play/pause

## Visual Style

| Property | Value |
|---|---|
| Background | `#0d0d10` |
| Sidebar | `#0e0e12` |
| Accent | `#6366f1` (indigo) |
| ML Agent avatar | `#7c3aed`, initials `MA` |
| Font | System UI / Inter |
| Cursor | White SVG arrow, CSS-transitioned |
| Click effect | Expanding ring ripple |
| Caption | Frosted glass pill, centered above controls bar |
