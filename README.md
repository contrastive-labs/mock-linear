# Mock Linear — ML Training Agent Demo

This repository contains an animated, screen-capture-style web demonstration of an automated Machine Learning workflow inside a simulated Linear app interface. 

The animation demonstrates how an AI agent handles a model training task end-to-end:
1. Receiving a new issue requesting a model architecture change (TransposeConv2d vs PixelShuffle).
2. Providing a cost and time estimate.
3. Launching a training job on AWS Batch.
4. Reporting back with detailed evaluation metrics (PSNR/SSIM) and a conclusion.

## Live Demo

The animation is deployed on GitHub Pages and can be viewed here:  
👉 **[https://contrastive-labs.github.io/mock-linear/](https://contrastive-labs.github.io/mock-linear/)**

## Repository Contents

- `index.html` — The main animated HTML/CSS/JS page. It features a custom playback engine, timeline scrubbing, and dynamic DOM manipulation to simulate typing, clicking, and scrolling.
- `spec.md` — The detailed design specification outlining the exact step-by-step sequence, visual styling, zoom behavior, and scroll logic used to build the animation.
- `skills/web-animation-design/SKILL.md` — A reusable agent skill file encoding the design rules and patterns (like targeted zoom and minimal scroll) established in this project.
