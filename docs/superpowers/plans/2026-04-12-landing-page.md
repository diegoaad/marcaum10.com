# Marcaum10 Landing Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a single-page marketing landing page for marcaum10.com with waitlist email capture.

**Architecture:** Astro v6 static site with Tailwind CSS v4 (via `@tailwindcss/vite`), deployed to Cloudflare Pages via GitHub Actions. Eight Astro components compose the page: Navbar, Hero, Problem, Features, Audience, Positioning, WaitlistForm, Footer. A base Layout component wraps them all with shared head/meta/fonts.

**Tech Stack:** Astro 6, Tailwind CSS 4, TypeScript, Cloudflare Pages, GitHub Actions, wrangler-action v3

**Spec:** `docs/superpowers/specs/2026-04-12-landing-page-design.md`

---

## File Structure

```
marcaum10.com/
├── .github/workflows/deploy.yml          — CI/CD to Cloudflare Pages
├── .gitignore                            — standard Astro ignores
├── astro.config.mjs                      — Astro config with Tailwind vite plugin
├── package.json                          — project deps and scripts
├── tsconfig.json                         — TypeScript config
├── public/
│   ├── marcaum10.png                     — logo (already exists)
│   └── favicon.svg                       — favicon derived from logo
├── src/
│   ├── styles/
│   │   └── global.css                    — Tailwind import + custom CSS vars
│   ├── layouts/
│   │   └── Layout.astro                  — base HTML shell (head, meta, fonts, body wrapper)
│   ├── components/
│   │   ├── Navbar.astro                  — sticky nav with logo + CTA
│   │   ├── Hero.astro                    — dark hero with headline + CTAs
│   │   ├── Problem.astro                 — light problem statement section
│   │   ├── Features.astro                — 6-card feature grid
│   │   ├── Audience.astro                — dark "who it's for" section
│   │   ├── Positioning.astro             — light differentiator copy
│   │   ├── WaitlistForm.astro            — dark section with email form
│   │   └── Footer.astro                  — minimal copyright footer
│   └── pages/
│       └── index.astro                   — page that composes all components
```

---

### Task 1: Create GitHub repo and initialize Astro project

**Files:**
- Create: `package.json`
- Create: `astro.config.mjs`
- Create: `tsconfig.json`
- Create: `.gitignore`
- Create: `src/pages/index.astro` (placeholder)

- [ ] **Step 1: Create the GitHub repo**

```bash
cd /home/diego/code/marcaum10.com
gh repo create diegoaad/marcaum10.com --public --description "Marcaum10 - Lifecycle orchestration platform" --source . --push=false
```

Expected: repo created at github.com/diegoaad/marcaum10.com

- [ ] **Step 2: Initialize git**

```bash
cd /home/diego/code/marcaum10.com
git init
```

- [ ] **Step 3: Create .gitignore**

Create `.gitignore`:

```
# build output
dist/
# generated types
.astro/

# dependencies
node_modules/

# logs
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*

# environment variables
.env
.env.production

# macOS-specific files
.DS_Store

# jetbrains setting folder
.idea/
```

- [ ] **Step 4: Initialize npm project and install dependencies**

```bash
cd /home/diego/code/marcaum10.com
npm init -y
npm pkg set name="marcaum10.com" type="module" version="0.0.1"
npm pkg set engines.node=">=22.12.0"
npm pkg set scripts.dev="astro dev" scripts.build="astro build" scripts.preview="astro preview" scripts.astro="astro"
npm install astro@^6.1.5
npm install tailwindcss@^4.2.2 @tailwindcss/vite@^4.2.2
```

- [ ] **Step 5: Create astro.config.mjs**

Create `astro.config.mjs`:

```javascript
// @ts-check
import { defineConfig } from 'astro/config';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  vite: {
    plugins: [tailwindcss()],
  },
});
```

- [ ] **Step 6: Create tsconfig.json**

Create `tsconfig.json`:

```json
{
  "extends": "astro/tsconfigs/strict",
  "include": [".astro/types.d.ts", "**/*"],
  "exclude": ["dist"]
}
```

- [ ] **Step 7: Create placeholder index page**

Create `src/pages/index.astro`:

```astro
---
---
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Marcaum10</title>
  </head>
  <body>
    <h1>Marcaum10</h1>
  </body>
</html>
```

- [ ] **Step 8: Verify the project builds**

```bash
cd /home/diego/code/marcaum10.com
npm run build
```

Expected: build succeeds, output in `dist/`

- [ ] **Step 9: Commit**

```bash
cd /home/diego/code/marcaum10.com
git add .gitignore package.json package-lock.json astro.config.mjs tsconfig.json src/pages/index.astro
git commit -m "chore: initialize Astro project with Tailwind CSS v4"
```

---

### Task 2: Set up Tailwind, global styles, and Layout component

**Files:**
- Create: `src/styles/global.css`
- Create: `src/layouts/Layout.astro`
- Create: `public/favicon.svg`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Move logo to public/**

```bash
cp /home/diego/code/marcaum10.com/marcaum10.png /home/diego/code/marcaum10.com/public/marcaum10.png
```

- [ ] **Step 2: Create favicon.svg**

Create `public/favicon.svg` — a simple hourglass icon in SVG:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <path d="M8 4h16v2H8zm0 22h16v2H8zm2-2h12c0-4-3-6-6-8 3-2 6-4 6-8H10c0 4 3 6 6 8-3 2-6 4-6 8z" fill="#0f1117"/>
</svg>
```

- [ ] **Step 3: Create global.css**

Create `src/styles/global.css`:

```css
@import "tailwindcss";

@theme {
  --color-dark: #0f1117;
  --color-dark-secondary: #1a1b26;
  --color-light: #ffffff;
  --color-light-secondary: #f8f9fa;
  --color-accent: #f59e0b;
  --color-accent-hover: #d97706;
  --color-text-dark: #1a1a2e;
  --color-text-muted: #9ca3af;

  --font-sans: "Inter", ui-sans-serif, system-ui, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
}
```

- [ ] **Step 4: Create Layout.astro**

Create `src/layouts/Layout.astro`:

```astro
---
interface Props {
  title?: string;
  description?: string;
}

const {
  title = "Marcaum10 — Lifecycle orchestration for long-running processes",
  description = "A durable lifecycle manager for long-running, asynchronous, and human-in-the-loop processes. Coordinate jobs, approvals, agents, and external events without idle workers.",
} = Astro.props;
---
<!doctype html>
<html lang="en" class="scroll-smooth">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="description" content={description} />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
    <title>{title}</title>
  </head>
  <body class="bg-dark text-white font-sans antialiased">
    <slot />
  </body>
</html>

<style>
  @import "../styles/global.css";
</style>
```

- [ ] **Step 5: Update index.astro to use Layout**

Replace `src/pages/index.astro`:

```astro
---
import Layout from "../layouts/Layout.astro";
---
<Layout>
  <main>
    <h1 class="text-4xl text-accent font-bold p-8">Marcaum10</h1>
    <p class="text-text-muted px-8">Landing page coming together...</p>
  </main>
</Layout>
```

- [ ] **Step 6: Verify build and dev server**

```bash
cd /home/diego/code/marcaum10.com
npm run build
```

Expected: build succeeds, Tailwind classes are applied in output HTML

- [ ] **Step 7: Commit**

```bash
git add public/marcaum10.png public/favicon.svg src/styles/global.css src/layouts/Layout.astro src/pages/index.astro
git commit -m "feat: add Layout component with Tailwind v4, global styles, and Inter font"
```

---

### Task 3: Build Navbar component

**Files:**
- Create: `src/components/Navbar.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Navbar.astro**

Create `src/components/Navbar.astro`:

```astro
---
---
<nav class="fixed top-0 left-0 right-0 z-50 bg-dark/90 backdrop-blur-sm border-b border-white/5">
  <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
    <a href="/" class="flex items-center gap-2">
      <img src="/marcaum10.png" alt="Marcaum10" class="h-8 w-8" />
      <span class="text-lg font-semibold text-white">Marcaum10</span>
    </a>
    <a
      href="#waitlist"
      class="bg-accent hover:bg-accent-hover text-dark font-semibold text-sm px-4 py-2 rounded-lg transition-colors"
    >
      Join Waitlist
    </a>
  </div>
</nav>
```

- [ ] **Step 2: Add Navbar to index.astro**

Replace `src/pages/index.astro`:

```astro
---
import Layout from "../layouts/Layout.astro";
import Navbar from "../components/Navbar.astro";
---
<Layout>
  <Navbar />
  <main class="pt-16">
    <h1 class="text-4xl text-accent font-bold p-8">Marcaum10</h1>
  </main>
</Layout>
```

- [ ] **Step 3: Verify build**

```bash
cd /home/diego/code/marcaum10.com
npm run build
```

Expected: build succeeds

- [ ] **Step 4: Commit**

```bash
git add src/components/Navbar.astro src/pages/index.astro
git commit -m "feat: add sticky Navbar with logo and waitlist CTA"
```

---

### Task 4: Build Hero component

**Files:**
- Create: `src/components/Hero.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Hero.astro**

Create `src/components/Hero.astro`:

```astro
---
---
<section class="bg-dark min-h-[80vh] flex items-center">
  <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-24 text-center">
    <h1 class="text-4xl sm:text-5xl lg:text-6xl font-bold text-white leading-tight mb-6">
      Pause the process.<br />Keep the progress.
    </h1>
    <p class="text-lg sm:text-xl text-text-muted max-w-2xl mx-auto mb-10 leading-relaxed">
      Marcaum10 is a lifecycle manager for long-running, asynchronous, and
      human-in-the-loop processes. Coordinate jobs, approvals, agents, and
      external events without keeping workers alive or building custom
      pause/resume logic.
    </p>
    <div class="flex flex-col sm:flex-row gap-4 justify-center items-center">
      <a
        href="#waitlist"
        class="bg-accent hover:bg-accent-hover text-dark font-semibold px-8 py-3 rounded-lg text-lg transition-colors"
      >
        Join the Waitlist
      </a>
      <a
        href="#problem"
        class="text-text-muted hover:text-white transition-colors text-lg flex items-center gap-1"
      >
        Learn more
        <svg class="w-4 h-4 mt-0.5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7" />
        </svg>
      </a>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add Hero to index.astro**

Replace `src/pages/index.astro`:

```astro
---
import Layout from "../layouts/Layout.astro";
import Navbar from "../components/Navbar.astro";
import Hero from "../components/Hero.astro";
---
<Layout>
  <Navbar />
  <main class="pt-16">
    <Hero />
  </main>
</Layout>
```

- [ ] **Step 3: Verify build**

```bash
cd /home/diego/code/marcaum10.com
npm run build
```

Expected: build succeeds

- [ ] **Step 4: Commit**

```bash
git add src/components/Hero.astro src/pages/index.astro
git commit -m "feat: add Hero section with headline and CTAs"
```

---

### Task 5: Build Problem section component

**Files:**
- Create: `src/components/Problem.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Problem.astro**

Create `src/components/Problem.astro`:

```astro
---
---
<section id="problem" class="bg-light py-20 sm:py-28">
  <div class="max-w-3xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="text-3xl sm:text-4xl font-bold text-text-dark mb-8">
      Long-running processes break the request/response model.
    </h2>
    <div class="space-y-4 text-lg text-text-dark/80 leading-relaxed">
      <p>
        Approvals take hours. External systems respond later. Agents need human
        input. Jobs run longer than a worker should stay alive.
      </p>
      <p>
        Most teams patch this together with polling, status tables, cron jobs,
        and retry spaghetti.
      </p>
    </div>
    <p class="mt-8 text-xl font-semibold text-text-dark">
      Marcaum10 gives you a durable lifecycle layer for pause, wait, resume,
      and route.
    </p>
  </div>
</section>
```

- [ ] **Step 2: Add Problem to index.astro**

Replace `src/pages/index.astro`:

```astro
---
import Layout from "../layouts/Layout.astro";
import Navbar from "../components/Navbar.astro";
import Hero from "../components/Hero.astro";
import Problem from "../components/Problem.astro";
---
<Layout>
  <Navbar />
  <main class="pt-16">
    <Hero />
    <Problem />
  </main>
</Layout>
```

- [ ] **Step 3: Verify build**

```bash
cd /home/diego/code/marcaum10.com
npm run build
```

Expected: build succeeds

- [ ] **Step 4: Commit**

```bash
git add src/components/Problem.astro src/pages/index.astro
git commit -m "feat: add Problem section"
```

---

### Task 6: Build Features section component

**Files:**
- Create: `src/components/Features.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Features.astro**

Create `src/components/Features.astro`:

```astro
---
const features = [
  {
    title: "Model states & transitions",
    description: "Define process lifecycles with a flexible finite-state-machine model.",
    icon: "M9 17V7m0 10a2 2 0 01-2 2H5a2 2 0 01-2-2V7a2 2 0 012-2h2a2 2 0 012 2m0 10a2 2 0 002 2h2a2 2 0 002-2M9 7a2 2 0 012-2h2a2 2 0 012 2m0 10V7",
  },
  {
    title: "Persist context durably",
    description: "Store process state and memory across restarts and deploys.",
    icon: "M4 7v10c0 2 1 3 3 3h10c2 0 3-1 3-3V7M4 7c0-2 1-3 3-3h10c2 0 3 1 3 3M4 7h16",
  },
  {
    title: "Pause for input",
    description: "Wait for human decisions, approvals, or external events — natively.",
    icon: "M10 9v6m4-6v6m7-3a9 9 0 11-18 0 9 9 0 0118 0z",
  },
  {
    title: "Resume from signals",
    description: "Resume from API calls, webhooks, timeouts, or manual action.",
    icon: "M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z M21 12a9 9 0 11-18 0 9 9 0 0118 0z",
  },
  {
    title: "Route next steps",
    description: "Direct execution to the right service or actor based on current state.",
    icon: "M13 7h8m0 0v8m0-8l-8 8-4-4-6 6",
  },
  {
    title: "Visible history",
    description: "Full audit trail of transitions, actors, and timing.",
    icon: "M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-6 9l2 2 4-4",
  },
];
---
<section class="bg-light-secondary py-20 sm:py-28">
  <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="text-3xl sm:text-4xl font-bold text-text-dark text-center mb-14">
      What it does
    </h2>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
      {features.map((feature) => (
        <div class="bg-white rounded-xl p-6 shadow-sm border border-gray-100">
          <div class="w-10 h-10 bg-accent/10 rounded-lg flex items-center justify-center mb-4">
            <svg class="w-5 h-5 text-accent" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" d={feature.icon} />
            </svg>
          </div>
          <h3 class="text-lg font-semibold text-text-dark mb-2">{feature.title}</h3>
          <p class="text-text-dark/70 leading-relaxed">{feature.description}</p>
        </div>
      ))}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add Features to index.astro**

Replace `src/pages/index.astro`:

```astro
---
import Layout from "../layouts/Layout.astro";
import Navbar from "../components/Navbar.astro";
import Hero from "../components/Hero.astro";
import Problem from "../components/Problem.astro";
import Features from "../components/Features.astro";
---
<Layout>
  <Navbar />
  <main class="pt-16">
    <Hero />
    <Problem />
    <Features />
  </main>
</Layout>
```

- [ ] **Step 3: Verify build**

```bash
cd /home/diego/code/marcaum10.com
npm run build
```

Expected: build succeeds

- [ ] **Step 4: Commit**

```bash
git add src/components/Features.astro src/pages/index.astro
git commit -m "feat: add Features section with 6-card grid"
```

---

### Task 7: Build Audience section component

**Files:**
- Create: `src/components/Audience.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Audience.astro**

Create `src/components/Audience.astro`:

```astro
---
const audiences = [
  {
    label: "Async workflows",
    description: "Coordinate multi-step processes that span services and time.",
  },
  {
    label: "Approval systems",
    description: "Model human decision points as durable waiting states.",
  },
  {
    label: "Long-running jobs",
    description: "Track jobs that outlive any single worker or deployment.",
  },
  {
    label: "AI/agent processes",
    description: "Pause agents for review, clarification, or tool results.",
  },
  {
    label: "Event-driven flows",
    description: "Resume from webhooks, callbacks, or external system signals.",
  },
];
---
<section class="bg-dark-secondary py-20 sm:py-28">
  <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
    <h2 class="text-3xl sm:text-4xl font-bold text-white text-center mb-14">
      Built for teams creating
    </h2>
    <div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
      {audiences.map((item) => (
        <div class="flex items-start gap-4 p-4">
          <div class="mt-1 w-2 h-2 rounded-full bg-accent shrink-0"></div>
          <div>
            <h3 class="text-lg font-semibold text-white mb-1">{item.label}</h3>
            <p class="text-text-muted leading-relaxed">{item.description}</p>
          </div>
        </div>
      ))}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add Audience to index.astro**

Replace `src/pages/index.astro`:

```astro
---
import Layout from "../layouts/Layout.astro";
import Navbar from "../components/Navbar.astro";
import Hero from "../components/Hero.astro";
import Problem from "../components/Problem.astro";
import Features from "../components/Features.astro";
import Audience from "../components/Audience.astro";
---
<Layout>
  <Navbar />
  <main class="pt-16">
    <Hero />
    <Problem />
    <Features />
    <Audience />
  </main>
</Layout>
```

- [ ] **Step 3: Verify build**

```bash
cd /home/diego/code/marcaum10.com
npm run build
```

Expected: build succeeds

- [ ] **Step 4: Commit**

```bash
git add src/components/Audience.astro src/pages/index.astro
git commit -m "feat: add Audience section"
```

---

### Task 8: Build Positioning section component

**Files:**
- Create: `src/components/Positioning.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Positioning.astro**

Create `src/components/Positioning.astro`:

```astro
---
---
<section class="bg-light py-20 sm:py-28">
  <div class="max-w-3xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
    <div class="space-y-4">
      <p class="text-2xl sm:text-3xl font-semibold text-text-dark/40">
        Not another worker sitting idle.
      </p>
      <p class="text-2xl sm:text-3xl font-semibold text-text-dark/55">
        Not another polling loop.
      </p>
      <p class="text-2xl sm:text-3xl font-semibold text-text-dark/70">
        Not another custom orchestration table.
      </p>
      <p class="text-2xl sm:text-3xl font-bold text-text-dark pt-4">
        A simpler control layer for processes that need to wait.
      </p>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add Positioning to index.astro**

Replace `src/pages/index.astro`:

```astro
---
import Layout from "../layouts/Layout.astro";
import Navbar from "../components/Navbar.astro";
import Hero from "../components/Hero.astro";
import Problem from "../components/Problem.astro";
import Features from "../components/Features.astro";
import Audience from "../components/Audience.astro";
import Positioning from "../components/Positioning.astro";
---
<Layout>
  <Navbar />
  <main class="pt-16">
    <Hero />
    <Problem />
    <Features />
    <Audience />
    <Positioning />
  </main>
</Layout>
```

- [ ] **Step 3: Verify build**

```bash
cd /home/diego/code/marcaum10.com
npm run build
```

Expected: build succeeds

- [ ] **Step 4: Commit**

```bash
git add src/components/Positioning.astro src/pages/index.astro
git commit -m "feat: add Positioning section"
```

---

### Task 9: Build WaitlistForm and Footer components

**Files:**
- Create: `src/components/WaitlistForm.astro`
- Create: `src/components/Footer.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create WaitlistForm.astro**

Create `src/components/WaitlistForm.astro`:

```astro
---
---
<section id="waitlist" class="bg-dark py-20 sm:py-28">
  <div class="max-w-xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
    <h2 class="text-3xl sm:text-4xl font-bold text-white mb-4">
      Interested in early access?
    </h2>
    <p class="text-text-muted text-lg mb-10">
      Sign up to follow the build.
    </p>

    <form id="waitlist-form" class="flex flex-col sm:flex-row gap-3 max-w-md mx-auto">
      <input
        type="email"
        name="email"
        required
        placeholder="you@company.com"
        class="flex-1 px-4 py-3 rounded-lg bg-white/10 border border-white/10 text-white placeholder-text-muted focus:outline-none focus:ring-2 focus:ring-accent focus:border-transparent"
      />
      <button
        type="submit"
        class="bg-accent hover:bg-accent-hover text-dark font-semibold px-6 py-3 rounded-lg transition-colors whitespace-nowrap"
      >
        Join Waitlist
      </button>
    </form>

    <div id="waitlist-success" class="hidden">
      <div class="bg-accent/10 border border-accent/20 rounded-lg p-6 max-w-md mx-auto">
        <p class="text-accent font-semibold text-lg">Thanks! We'll be in touch.</p>
      </div>
    </div>
  </div>
</section>

<script>
  const form = document.getElementById("waitlist-form") as HTMLFormElement;
  const success = document.getElementById("waitlist-success") as HTMLDivElement;
  const apiUrl = import.meta.env.PUBLIC_WAITLIST_API_URL;

  form.addEventListener("submit", async (e) => {
    e.preventDefault();
    const formData = new FormData(form);
    const email = formData.get("email") as string;

    if (apiUrl) {
      try {
        await fetch(apiUrl, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ email }),
        });
      } catch {
        // Endpoint not ready yet — show success anyway
      }
    }

    form.classList.add("hidden");
    success.classList.remove("hidden");
  });
</script>
```

- [ ] **Step 2: Create Footer.astro**

Create `src/components/Footer.astro`:

```astro
---
---
<footer class="bg-dark border-t border-white/5 py-8">
  <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
    <p class="text-text-muted text-sm">&copy; 2026 Marcaum10</p>
  </div>
</footer>
```

- [ ] **Step 3: Update index.astro with all components**

Replace `src/pages/index.astro`:

```astro
---
import Layout from "../layouts/Layout.astro";
import Navbar from "../components/Navbar.astro";
import Hero from "../components/Hero.astro";
import Problem from "../components/Problem.astro";
import Features from "../components/Features.astro";
import Audience from "../components/Audience.astro";
import Positioning from "../components/Positioning.astro";
import WaitlistForm from "../components/WaitlistForm.astro";
import Footer from "../components/Footer.astro";
---
<Layout>
  <Navbar />
  <main class="pt-16">
    <Hero />
    <Problem />
    <Features />
    <Audience />
    <Positioning />
    <WaitlistForm />
  </main>
  <Footer />
</Layout>
```

- [ ] **Step 4: Verify build**

```bash
cd /home/diego/code/marcaum10.com
npm run build
```

Expected: build succeeds

- [ ] **Step 5: Commit**

```bash
git add src/components/WaitlistForm.astro src/components/Footer.astro src/pages/index.astro
git commit -m "feat: add WaitlistForm and Footer, complete page composition"
```

---

### Task 10: Set up GitHub Actions deploy workflow and Cloudflare Pages project

**Files:**
- Create: `.github/workflows/deploy.yml`

- [ ] **Step 1: Create the Cloudflare Pages project**

Use the Cloudflare MCP to check the account and create the project, or run:

```bash
npx wrangler pages project create marcaum10-com --production-branch main
```

- [ ] **Step 2: Create deploy.yml**

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Cloudflare Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm

      - run: npm ci

      - run: npm run build

      - uses: cloudflare/wrangler-action@v3
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          accountId: bebf135984d76ba50483560787bbec91
          command: pages deploy dist --project-name marcaum10-com --branch main
```

- [ ] **Step 3: Verify the CLOUDFLARE_API_TOKEN secret exists for the new repo**

```bash
gh secret list --repo diegoaad/marcaum10.com
```

If the secret is not set, it needs to be configured. Check if it's set at the org/user level or copy from the existing repo.

- [ ] **Step 4: Commit**

```bash
git add .github/workflows/deploy.yml
git commit -m "ci: add Cloudflare Pages deploy workflow"
```

---

### Task 11: Push to GitHub and verify deployment

- [ ] **Step 1: Add remote and push**

```bash
cd /home/diego/code/marcaum10.com
git remote add origin https://github.com/diegoaad/marcaum10.com.git
git branch -M main
git push -u origin main
```

- [ ] **Step 2: Verify GitHub Action runs**

```bash
gh run list --repo diegoaad/marcaum10.com --limit 1
```

Wait for the run to complete:

```bash
gh run watch --repo diegoaad/marcaum10.com
```

Expected: workflow completes successfully, site deployed to Cloudflare Pages

- [ ] **Step 3: Verify the deployed site**

Check that the site is accessible at the Cloudflare Pages URL (e.g., `marcaum10-com.pages.dev`) and all sections render correctly.

- [ ] **Step 4: Set up custom domain**

Configure `marcaum10.com` to point to the Cloudflare Pages deployment via the Cloudflare dashboard or CLI. Since the domain is already on Cloudflare, this is a CNAME record pointing to `marcaum10-com.pages.dev`.
