# Marcaum10 Landing Page — Design Spec

## Overview

A single-page marketing landing page for marcaum10.com. Static site built with Astro + Tailwind CSS, deployed to Cloudflare Pages via GitHub Actions.

## Goal

Ship a credible landing page that communicates what Marcaum10 is, who it's for, and captures waitlist signups. No backend — the waitlist form posts to a configurable endpoint (to be built separately on the user's VPS).

## Tech Stack

- **Framework:** Astro (latest v6, matching existing diegoduarte.cloud setup)
- **Styling:** Tailwind CSS
- **Deployment:** Cloudflare Pages via GitHub Actions (wrangler-action@v3)
- **Node:** 22
- **Repo:** `diegoaad/marcaum10.com` on GitHub

## GitHub Actions Deployment

Workflow triggers on push to `main` and `workflow_dispatch`. Uses the same pattern as `diegoduarte.cloud`:

- `actions/checkout@v4`
- `actions/setup-node@v4` with Node 22 and npm cache
- `npm ci` + `npm run build`
- `cloudflare/wrangler-action@v3` with:
  - `apiToken`: `${{ secrets.CLOUDFLARE_API_TOKEN }}` (same secret already in the GitHub org/user)
  - `accountId`: `bebf135984d76ba50483560787bbec91` (same Cloudflare account)
  - `command`: `pages deploy dist --project-name marcaum10-com --branch main`

The Cloudflare Pages project `marcaum10-com` needs to be created first (via MCP/dashboard or wrangler).

## Page Structure

All content lives in a single Astro page (`src/pages/index.astro`) composed from components in `src/components/`.

### 1. Navbar

- Sticky top bar
- Logo: the hourglass PNG (`marcaum10.png`) + "Marcaum10" text
- Single CTA button: "Join Waitlist" — smooth scrolls to the waitlist form section
- Mobile: logo + CTA button only (no hamburger — single page, no nav links to collapse)

### 2. Hero Section

- **Background:** Dark (charcoal/near-black)
- **Heading:** "Pause the process. Keep the progress."
- **Subtext:** "Marcaum10 is a lifecycle manager for long-running, asynchronous, and human-in-the-loop processes. Coordinate jobs, approvals, agents, and external events without keeping workers alive or building custom pause/resume logic."
- **CTA:** "Join the Waitlist" button, scrolls to waitlist form
- **Secondary CTA:** "Learn more" or down-arrow, scrolls to problem section

### 3. Problem Section

- **Background:** Light (white or very light gray)
- **Heading:** "Long-running processes break the request/response model."
- **Body copy:** Approvals take hours. External systems respond later. Agents need human input. Jobs run longer than a worker should stay alive. Most teams patch this together with polling, status tables, cron jobs, and retry spaghetti.
- **Closing line:** "Marcaum10 gives you a durable lifecycle layer for pause, wait, resume, and route."

### 4. What It Does (Features)

- **Background:** Subtle contrast (very light gray or white, opposite of problem section)
- **Heading:** "What it does"
- **Layout:** 6 feature cards in a responsive grid (3x2 on desktop, 2x3 on tablet, 1x6 on mobile)
- Cards:
  1. **Model states & transitions** — Define process lifecycles with a flexible FSM model
  2. **Persist context durably** — Store process state and memory across restarts and deploys
  3. **Pause for input** — Wait for human decisions, approvals, or external events
  4. **Resume from signals** — Resume from API calls, webhooks, timeouts, or manual action
  5. **Route next steps** — Direct execution to the right service or actor based on state
  6. **Visible history** — Full audit trail of transitions, actors, and timing

### 5. Who It's For

- **Background:** Dark section (matches hero tone)
- **Heading:** "Built for teams creating"
- **Layout:** List or card layout with:
  - Async workflows
  - Approval systems
  - Long-running jobs
  - AI/agent processes
  - External event-driven flows

### 6. Positioning Block

- **Background:** Light
- **Copy (large/styled text):**
  - "Not another worker sitting idle."
  - "Not another polling loop."
  - "Not another custom orchestration table."
  - "A simpler control layer for processes that need to wait."

### 7. Waitlist Form Section

- **Background:** Dark
- **Heading:** "Interested in early access?"
- **Subtext:** "Sign up to follow the build."
- **Form:** Single email input + "Join Waitlist" submit button
- **Behavior:**
  - Posts to a configurable API endpoint (environment variable `PUBLIC_WAITLIST_API_URL`)
  - If no endpoint is configured or request fails, shows a client-side "Thanks! We'll be in touch." message anyway (graceful degradation until backend is ready)
  - On success, shows confirmation message replacing the form
- **Secondary:** "Or email us at [contact email]" link

### 8. Footer

- Copyright line: "© 2026 Marcaum10"
- Minimal — no nav links for now

## Color Palette

- **Dark sections:** `#0f1117` or similar charcoal (hero, who it's for, waitlist)
- **Light sections:** `#ffffff` / `#f8f9fa` (problem, features, positioning)
- **Accent:** Warm amber/gold (e.g., `#f59e0b` or similar) for CTA buttons and highlights — evokes the hourglass/sand theme from the logo
- **Text:** White on dark, `#1a1a2e` or near-black on light
- **Secondary text:** Muted gray variants for subtitles

## Typography

- System font stack or a clean sans-serif from Google Fonts (e.g., Inter)
- Large hero heading (4xl-6xl), clear hierarchy down through sections

## Assets

- `marcaum10.png` — logo, used in navbar and potentially favicon (will generate a favicon from it)

## Project Structure

```
marcaum10.com/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── public/
│   ├── marcaum10.png
│   └── favicon.svg (or .ico, derived from logo)
├── src/
│   ├── components/
│   │   ├── Navbar.astro
│   │   ├── Hero.astro
│   │   ├── Problem.astro
│   │   ├── Features.astro
│   │   ├── Audience.astro
│   │   ├── Positioning.astro
│   │   ├── WaitlistForm.astro
│   │   └── Footer.astro
│   ├── layouts/
│   │   └── Layout.astro
│   └── pages/
│       └── index.astro
├── astro.config.mjs
├── tailwind.config.mjs (if needed beyond Astro defaults)
├── tsconfig.json
├── package.json
└── .gitignore
```

## What's NOT Included

- No backend API for waitlist storage (separate VPS project)
- No multi-page routing (future expansion)
- No animations or complex interactions
- No analytics or tracking
- No blog or docs content
- No dark/light mode toggle (fixed dark hero + light body pattern)

## Environment Variables

- `PUBLIC_WAITLIST_API_URL` — endpoint for waitlist form submission (optional, form degrades gracefully without it)

## Open Items

- Contact email address to display (or omit until decided)
- Custom domain DNS setup on Cloudflare (user already owns marcaum10.com)
