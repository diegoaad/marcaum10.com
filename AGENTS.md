# Repository Guidelines

## Project Identity and Ultimate Goals

Marcaum10 is planned as a lifecycle orchestration platform for long-running, asynchronous, human-in-the-loop, and agent-driven processes.

The ultimate product goal is to build the modern control plane for long-running processes and agents. Marcaum10 should help developers coordinate workflows that need to pause, wait, resume, route, and persist context across humans, services, webhooks, timeouts, and agents without keeping workers alive or relying on brittle polling loops.

The business goal is to validate that technical teams will pay for a developer-friendly platform that simplifies long-running process coordination. The current website is the validation front-end for that larger product.

When making product, copy, architecture, or roadmap decisions, use these source documents first:

- `docs/marcaum10.md` for the full launch pack, PRD, business plan, and starter copy.
- `docs/marcaum10-project-goals.md` for the distilled goals.
- `docs/marcaum10-roadmap.md` for the current project plan and roadmap.

## Current Repository Scope

This repository currently contains the Astro landing page for `marcaum10.com`. Its near-term job is to publish a credible site that explains the concept, captures waitlist interest, and supports customer discovery.

Do not assume the future platform backend, SDK, dashboard, or lifecycle engine exists in this repo yet. If adding those later, update this file and the roadmap in the same change.

## Future Product Scope

Future Marcaum10 work should move toward these product capabilities:

- Process definitions with states, transitions, terminal states, metadata, and timeouts.
- Process instances with durable context, correlation IDs, and external references.
- Transition API with expected-state checks, actor metadata, reasons, and context updates.
- Validation hooks that keep business rules owned by application services.
- Pause, wait, resume, timeout, signal, webhook, and manual operator primitives.
- Durable process history, observability, auditability, and stuck-process visibility.
- Routing to downstream workers or services based on process type and state.
- One primary SDK or starter client before expanding language support.
- A lightweight dashboard for process inspection and debugging.

Favor small, composable lifecycle primitives over a broad BPM or visual workflow-builder surface.

## Roadmap Principles

Build in this order unless the roadmap is deliberately revised:

1. Validate positioning through the landing page, waitlist, interviews, and practical examples.
2. Choose one sample workflow demo that proves pause/resume value clearly.
3. Define the core process data model and transition/resume API.
4. Build the MVP lifecycle engine with durable state, history, wait states, signals, and timeouts.
5. Add a lightweight dashboard, SDK, docs, and sample apps.
6. Onboard design partners before expanding enterprise features.

Early success metrics are time to first working process, weekly process creation, successful resume rate, reduction in polling/custom orchestration, and design partner satisfaction.

## Project Structure and Module Organization

Page composition starts in `src/pages/index.astro`, which imports the shared shell from `src/layouts/Layout.astro` and section components from `src/components/`.

Global Tailwind v4 theme tokens live in `src/styles/global.css`. Static assets that must be served directly belong in `public/`, such as `public/favicon.svg` and `public/marcaum10.png`.

Planning notes, specs, and implementation plans are kept under `docs/`. Superpowers planning artifacts live under `docs/superpowers/`.

## Build, Test, and Development Commands

- `npm ci` installs dependencies from `package-lock.json`; use it for clean local or CI setup.
- `npm run dev` starts the Astro development server.
- `npm run build` builds the production site into `dist/`; run this before opening a PR.
- `npm run preview` serves the built `dist/` output locally.
- `npm run astro -- <command>` runs Astro CLI commands, for example `npm run astro -- check` if type checking is needed.

`npm test` is currently a placeholder that exits with an error, so do not treat it as a validation command until a real test script is added.

## Coding Style and Naming Conventions

Use Astro single-file components with PascalCase filenames in `src/components/` such as `Hero.astro` and `WaitlistForm.astro`. Keep route files in `src/pages/` lowercase and route-oriented.

Follow the existing style: two-space indentation, double quotes in frontmatter/scripts, semicolons in JavaScript/TypeScript, and utility-first Tailwind classes in markup.

Add shared design tokens in `src/styles/global.css` instead of scattering hard-coded palette changes. Keep page sections clear, responsive, and consistent with the current quiet technical-product tone.

## Product Writing Guidance

Marcaum10 should be described as a lifecycle manager or durable control plane for long-running processes that need to wait and resume reliably.

Prefer concrete language around:

- Long-running workflows without long-running workers.
- Pause, wait, resume, route, and persist context.
- Human approval flows, async jobs, external callbacks, and agent workflows.
- Service-owned validation and business rules.
- Explicit process history and debuggability.

Avoid positioning the product as a full BPM suite, a visual workflow builder, or a replacement for every workflow engine. The wedge is durable lifecycle orchestration between simple queues and heavyweight workflow platforms.

## Testing Guidelines

There is no configured test framework yet. For current website changes, validate with `npm run build` and manually inspect key responsive states in `npm run dev` or `npm run preview`.

If adding behavior-heavy client scripts, data handling, API clients, or future platform code, add appropriate automated tests and replace the placeholder `npm test` script in the same change.

For future platform work, tests should cover idempotent transition handling, optimistic concurrency, valid and invalid state transitions, signal-based resume, timeout behavior, durable history, and authorization boundaries.

## Commit and Pull Request Guidelines

Recent history uses Conventional Commit prefixes such as `feat:`, `fix:`, and `ci:`. Keep subjects short and imperative, for example `feat: add audience section`.

Pull requests should include a summary, validation steps, linked issue if applicable, and screenshots or recordings for visual changes. For product or roadmap changes, include the source document or decision that motivated the change.

## Security and Configuration Tips

The waitlist form reads `PUBLIC_WAITLIST_API_URL`, so only expose values intended for the browser. Deployment runs through Cloudflare Pages in `.github/workflows/deploy.yml`; keep secrets such as `CLOUDFLARE_API_TOKEN` in GitHub Actions secrets, not in source files.

Future platform work must treat process context as potentially sensitive customer data. Design toward tenant isolation, auditability, least-privilege credentials, idempotent external callbacks, signed webhooks, and careful retention of process history.
