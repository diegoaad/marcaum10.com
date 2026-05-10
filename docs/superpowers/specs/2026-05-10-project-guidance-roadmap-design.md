# Marcaum10 Project Guidance and Roadmap Design

## Purpose

Rewrite `AGENTS.md` so future agents understand both the current Astro landing page and the larger Marcaum10 product direction. Add a separate roadmap document for the fuller project plan so `AGENTS.md` stays usable as day-to-day repo guidance.

## Approved Approach

Use a product-aware repo guide:

- `AGENTS.md` covers repository rules, current commands, coding style, validation requirements, security guidance, and the product north star.
- `docs/marcaum10-roadmap.md` covers the project plan and roadmap from landing page validation through MVP beta and early commercialization.
- The roadmap includes the future Marcaum10 platform beyond this repo: lifecycle API, control plane, runtime/event layer, persistence, SDK, dashboard, sample apps, design partners, and beta readiness.

## Scope

In scope:

- Rewrite `AGENTS.md`.
- Add `docs/marcaum10-roadmap.md`.
- Keep references aligned with `docs/marcaum10.md` and `docs/marcaum10-project-goals.md`.
- Preserve existing Astro build, test, style, and deployment guidance.

Out of scope:

- Implementing the future platform.
- Changing landing page code.
- Adding tests or tooling.
- Making deployment or CI changes.

## Success Criteria

- A future agent can identify the ultimate goal of Marcaum10 before editing code or copy.
- The current repo remains clearly described as an Astro landing page and validation front-end.
- The roadmap separates validation, MVP design, MVP build, private alpha, and beta readiness.
- No guidance implies that future platform components already exist in this repository.
