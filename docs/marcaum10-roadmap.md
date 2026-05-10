# Marcaum10 Project Plan and Roadmap

Sources:

- `docs/marcaum10.md`
- `docs/marcaum10-project-goals.md`

## North Star

Marcaum10 should become a durable lifecycle control plane for long-running processes and agents.

The product should help teams define process states, persist context, wait safely, resume reliably, route the next action, and inspect history without tying work to a single live process, idle worker, or polling loop.

## Strategic Positioning

Marcaum10 sits between simple queues and heavyweight workflow platforms.

It should be:

- Lightweight and developer-friendly.
- Lifecycle-first rather than BPM-heavy.
- Explicit about pause, wait, resume, signal, timeout, and route behavior.
- Friendly to human-in-the-loop and agentic workflows.
- Incrementally adoptable alongside existing services.
- Clear that domain services still own business validation and business rules.

## Success Metrics

Early validation and MVP success should be measured by:

- Time to first working process.
- Number of processes created weekly.
- Successful resume rate.
- Reduction in polling loops or custom orchestration code.
- Developer satisfaction from design partners.
- Waitlist signups and qualified user interviews during the validation phase.

## Phase 0: Foundation and Positioning

**Status:** Current repository focus.

**Goal:** Publish a credible landing page and sharpen the message before building too much product surface.

**Deliverables:**

- One-line pitch.
- Problem statement.
- Website copy.
- Brand/logo direction.
- Waitlist capture.
- Product architecture sketch.
- Project goals summary.
- Roadmap document.

**Implementation focus in this repo:**

- Keep the Astro landing page clear, fast, and credible.
- Align copy with the launch pack.
- Make the waitlist path obvious.
- Add or improve concept visuals only when they clarify pause/resume orchestration.
- Validate every site change with `npm run build`.

**Exit criteria:**

- Landing page is live.
- Waitlist capture is functional or gracefully degraded.
- The site clearly explains who Marcaum10 is for and what pain it solves.
- At least one architecture memo or concept note exists.
- Interview targets are identified.

## Phase 1: Validation and MVP Spec

**Goal:** Turn the concept into a concrete MVP with a narrow first use case.

**Deliverables:**

- Finalized PRD for MVP scope.
- Core domain model.
- API draft.
- Event/state lifecycle model.
- One selected demo use case.
- Interview notes and feedback synthesis.

**Key decisions:**

- Which use case comes first: approval workflow, external callback job, or agent review/resume.
- How strict process definitions should be.
- Where transition validation lives.
- Whether routing is native in the MVP or integration-driven.
- How large process context can be.
- What API ergonomics agents need.

**Recommended first demo:**

Use a human approval workflow with pause and later resume. It is easier for early users to understand than a fully agentic demo, while still proving the core lifecycle primitives.

**Exit criteria:**

- A developer can read the spec and understand how to create, wait, signal, resume, and inspect a process.
- The first demo can be described end to end without hidden platform behavior.
- MVP scope excludes visual workflow builders, BPMN modeling, enterprise RBAC, integration marketplaces, and complex multi-region operations.

## Phase 2: Lifecycle Engine MVP

**Goal:** Implement the smallest end-to-end lifecycle engine that proves durable state, transition history, waiting, resume, and timeouts.

**Core capabilities:**

- Process type registration.
- Process instance creation.
- Transition request API.
- Optimistic concurrency on state changes.
- Durable state and context persistence.
- Durable transition history.
- Waiting state support.
- Timeout scheduler.
- Resume by signal or event.
- Process detail view or API-first history inspection.

**Suggested architecture:**

- Control plane for process definitions, process instances, transition ledger, status queries, and future tenancy.
- Runtime/event layer for resume signals, timeouts, next-action dispatch, and lifecycle events.
- Persistence layer for current state snapshots, context blobs, history events, status indexes, and correlation IDs.
- Developer interfaces through REST API, webhook endpoints, and one primary SDK.
- UI/console for process explorer, status dashboard, event timeline, waiting reasons, and stuck processes.

**Suggested technical direction:**

- Backend: TypeScript or Kotlin.
- Database: Postgres.
- Queue/events: choose the simplest managed queue or DB-backed event loop that supports the first hosted MVP.
- Scheduler/timeouts: durable DB-backed scheduler first unless a managed scheduler is faster.
- Frontend: keep this Astro repo for marketing; use a separate app only when the dashboard needs authenticated product UI.

**Exit criteria:**

- One sample process runs from creation through wait, signal, resume, and terminal state.
- Invalid transitions are rejected safely.
- Duplicate signals or retries do not corrupt state.
- Operators can inspect current state, context summary, waiting reason, and history.

## Phase 3: Developer Experience and Samples

**Goal:** Make the MVP understandable and usable by design partners.

**Deliverables:**

- One SDK or starter client.
- API documentation.
- Quickstart guide.
- Sample app for the first workflow demo.
- Example templates for approval, external callback, and agent review flows.
- Lightweight dashboard improvements.

**Developer experience requirements:**

- A new user can create a first process quickly.
- API concepts map directly to state, transition, wait, signal, timeout, and history.
- Errors explain expected state, actual state, invalid transition, duplicate request, or missing signal clearly.
- Docs show service-owned validation rather than burying business rules inside Marcaum10.

**Exit criteria:**

- A design partner can run the sample app.
- A design partner can model one real workflow with minimal support.
- Feedback identifies platform boundaries more clearly.

## Phase 4: Private Alpha

**Goal:** Test the MVP with a small number of friendly users and refine reliability, usability, and messaging.

**Deliverables:**

- 3 to 5 design partners.
- Bug backlog.
- Feedback synthesis.
- Improved docs and onboarding.
- Reliability fixes around retries, stuck waits, signals, and timeout handling.

**Focus areas:**

- Where developers get confused in the API model.
- Which state/history views operators actually need.
- How often service-owned validation feels natural or awkward.
- Whether routing should become a native primitive.
- Whether agent workflows need additional context or checkpoint semantics.

**Exit criteria:**

- Design partners complete real or realistic workflows.
- Resume reliability is measurable.
- The product wedge is clearer than the generic workflow-engine category.

## Phase 5: Public Beta Readiness

**Goal:** Prepare Marcaum10 for broader public beta and early commercial packaging.

**Deliverables:**

- Improved landing page.
- Product documentation.
- Simple pricing page.
- Reference architecture.
- Case studies or design partner stories.
- Technical blog post.
- Launch checklist.

**Commercial packaging candidates:**

- Developer/startup tier with free or low-cost usage.
- Growth tier based on workflow executions, active processes, transitions, or event volume.
- Enterprise tier later with SLA, SSO, audit, RBAC, private networking, and support.

**Exit criteria:**

- Public beta users can self-serve onboarding.
- Pricing can be explained without a sales call.
- Differentiation against Temporal, Step Functions, Durable Functions, Inngest, Trigger.dev, and custom queue/state-machine systems is explicit.

## P0 Product Backlog

- Process type registration.
- Process instance creation.
- Transition API.
- Optimistic concurrency on state changes.
- Durable history.
- Waiting state support.
- Timeout scheduler.
- Resume by signal/event.
- Process detail page.

## P1 Product Backlog

- Correlation search.
- Retries and dead-letter handling.
- Webhook integrations.
- SDK helpers.
- Operator manual intervention tools.

## P2 Product Backlog

- Process templates.
- Dashboard metrics.
- Multi-tenant controls.
- RBAC.
- Audit export.
- Billing and usage metering.

## Immediate Next Actions

1. Keep improving the landing page around the clearest positioning: long-running workflows without long-running workers.
2. Choose the first sample workflow demo.
3. Draft the core process data model.
4. Draft the transition and resume API.
5. Interview 10 to 20 target users and validate the language before expanding the platform scope.
