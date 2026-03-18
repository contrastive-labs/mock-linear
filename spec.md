# ML Training Agent — Animation Design Spec

## Overview

A full-viewport animated web page that simulates an ML engineer creating a Linear issue and watching an AI agent autonomously handle the training job end-to-end. The animation plays automatically and includes zoom-ins on key text areas so viewers can read details clearly.

---

## Application Shell

The animation renders a faithful replica of the **Linear** project management app in dark mode, filling the entire browser viewport. The layout consists of:

- **Left sidebar** — workspace name, nav items (Inbox, Issues, Projects), and team list (ML Agents, Backend, Data Science)
- **Main content area** — issues list with existing tickets (ML-041, ML-042, ML-043)
- **Top bar** — breadcrumb showing `Issues › All Issues` and a `New Issue` button

---

## Animation Steps

Each step has a label shown in the **Step Indicator** bar at the bottom of the page.

### Step 1 — Opening Linear — Issues view
The app fades in from the intro splash screen. The cursor appears in the issues list area.

### Step 2 — Moving to New Issue button
The cursor animates from the issues list to the **New Issue** button in the top-right corner.

### Step 3 — Clicking New Issue
The cursor clicks the button. A click ripple effect plays.

### Step 4 — New Issue dialog opens
A modal dialog slides in with a **Title** input and **Description** textarea. Dialog window is 80% height and width of full window.

### Step 5 — Zoom in
The viewport zooms into to fit dialog window, so the text is clearly readable.

### Step 6 — Typing issue title
The title is typed character-by-character:
> `Try transposeconv2d rather than pixelshuffel for upsampling`


### Step 8 — Typing issue description
The description is typed character-by-character:
> `Can you please replace the upsampling layer with transpose conv2d and see how the test loss changes after training for 10 epochs? Please compare it to the baseline, which is pixel shuffle. `


### Step 10 — Submitting the issue
The cursor moves to the **Create Issue** button and clicks it. The dialog closes. Issue **ML-044** appears in the issues list.

### Step 11 — Opening the issue
The cursor clicks on ML-044 in the list. The issue detail panel slides open showing the title, description, and metadata sidebar. The issue details panel is 80% of width and height. 

### Step 12 — Zoom in
The viewport zooms into to fit issue details panel.


### Step 14 — Assigning to ML Agent
The cursor moves to the **Assignee** field in the metadata sidebar. Click and types in ML, and typeahead list hints ML Agent. The assignee is set to **ML Agent** (purple avatar chip appears).

### Step 15 — ML Agent reviews the issue
A caption reads: *"ML Agent is reviewing the issue…"*. A brief pause simulates the agent processing.

### Step 16 — Typing indicator appears
Three animated dots appear below the latest comment, indicating ML Agent is composing a comment.

### Step 17 — ML Agent posts estimate comment
The typing indicator is replaced by the first comment from ML Agent:

```
I checked out the repo and find all what I need.
I will run a training job on AWS Batch with 1 GPU.

Cost estimation: $3
ETA: 4 hours
```


### Step 19 — Agent started
ML Agent adds comment:
```
I launched a training job on AWS Batch.
- View job log (hyperlink)
- Monitor in Weights&Bias (link)
- Review my PR (link)
```
A caption reads: *"Training running on AWS Batch…"*

### Step 20 — ML Agent posts results comment
One more comment from ML Agent appears:

```
Here is what I found.

Training completed successfully on AWS Batch (g4dn.xlarge, 1× NVIDIA T4).

Results — super-resolution model (4× upscale):
  Baseline (8 layers):    PSNR 28.4 dB | SSIM 0.847
  +2 layers (10 total):   PSNR 29.1 dB | SSIM 0.861  ↑ +0.7 dB
  +4 layers (12 total):   PSNR 29.8 dB | SSIM 0.874  ↑ +1.4 dB

Recommendation: 12-layer model offers the best quality/cost trade-off.
Checkpoint saved to s3://ml-models/super-res/v3-12layers.pt

Next steps: run perceptual loss fine-tuning for another +0.3–0.5 dB gain.
```


---

## Zoom Behavior

- **Zoom in**: should fit the dialog window.
- **Zoom out**: back to normal full window.
- **Scroll**: When a new comment or the typing indicator appears, scroll only if it is **not fully visible** in the panel. When scrolling, scroll **minimally** — just enough so the bottom of the element aligns with the bottom of the visible area. Never center the element. If it is already fully in view, do not scroll.
- **Typing indicator placement**: The "ML Agent is typing…" indicator always appears **immediately below the last visible comment**. It is moved in the DOM to sit directly after the last comment with class `visible` before being shown.
- The **controls bar** (step indicator + playback row) is placed **outside** the zoom layer and is never scaled.
- The **caption overlay** and **cursor** are also outside the zoom layer.

---

## Playback Controls

The controls bar is fixed to the bottom of the viewport and contains two rows:

### Row 1 — Step Indicator
- **Step badge**: `Step X / 21` in indigo, pill-shaped
- **Step description**: current step label in white/light gray, 14px medium weight

### Row 2 — Playback Row
| Element | Description |
|---|---|
| ↺ Startover | Restarts the entire animation from the beginning |
| ⏮ Prev step | Jumps to the start of the previous step and auto pause |
| ▶ / ⏸ Play/Pause | Toggles playback |
| ⏭ Next step | Jumps to the start of the next step and auto pause |
| Progress bar | Draggable scrubber with white tick marks at each step boundary |
| Timestamp | `M:SS / M:SS` elapsed / total |
| Speed buttons | `0.5×` `1×` `1.5×` `2×` — sets playback speed multiplier |

### Keyboard Shortcuts
| Key | Action |
|---|---|
| `Space` | Toggle play / pause |
| `←` Arrow Left | Jump to previous step and auto pause |
| `→` Arrow Right | Jump to next step and auto pause |

---

## Visual Style

| Property | Value |
|---|---|
| Background | `#0d0d10` (near-black) |
| Sidebar background | `#0e0e12` |
| Issue row hover | `rgba(255,255,255,0.04)` |
| Accent color | `#6366f1` (indigo) |
| ML Agent avatar | Purple `#7c3aed` with initials `MA` |
| Font | System UI / Inter |
| Cursor | White SVG arrow with dark outline, animated with CSS transitions |
| Click effect | Expanding ring ripple at click point |
| Caption | Frosted glass pill, centered above controls bar |

---

## File Structure

```
ml-agent-animation/
├── index.html   # Single-file animation (HTML + CSS + JS)
└── spec.md      # This design specification
```
