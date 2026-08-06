# Javier Lucia Marco

Backend and full-stack engineer. I build and run production systems end to end —
API, workers, database, deployment pipeline and the client that talks to them.

Most of my repositories are private because they are commercial products in use.
This page describes what they do, how they are built and what I owned. Happy to
walk through any of them in detail, or share code under NDA.

📍 Zaragoza, Spain · j.luciamarco@gmail.com

---

## iFlySEO — AI content platform for WordPress

**Live product.** A multi-tenant SaaS that analyses a customer's website, infers
their service lines, and generates and publishes SEO content on a schedule. Sold
through a WooCommerce storefront with per-domain word accounting.

**Stack:** Python 3.11 · FastAPI · SQLAlchemy 2 (async) · Alembic · Celery ·
Redis · PostgreSQL 15 · Docker Compose · GitHub Actions · PHP 8 (WordPress
plugins) · vanilla JS

**What I built and run:**

- **Async API and worker fleet.** FastAPI service plus four Celery queues
  (generation, scraping, publishing, scheduling) behind Redis, with leased
  tasks, fenced completion and heartbeats so a dead worker cannot strand a job.
- **Content generation pipeline.** LLM generation with a validation layer —
  hard and soft constraints, candidate scoring, bounded retries — and an
  auditable record of every paid provider response. Cut wasted paid retries from
  8 per four articles to 2 by measuring the failures rather than guessing.
- **Two WordPress plugins in PHP.** A customer-site plugin (generation,
  scheduling, publishing, signed auto-update, an Ed25519-signed kill switch) and
  a Hub plugin that runs the storefront, customer portal and WooCommerce
  provisioning bridge.
- **Signed API boundary.** HMAC-SHA256 request signing with timestamp, nonce and
  body digest; encrypted secret storage; per-key rate limiting and lockout.
- **Deployment pipeline.** GitHub Actions: format, tests, PHP linting against
  each plugin's declared version, Semgrep, immutable image tags, health-gated
  deploy with automatic rollback, semantic versioning derived from commit type,
  and image cleanup that keeps the VPS disk in check.
- **Operational ownership.** VPS on Docker Compose with Prometheus and Grafana,
  tamper-evident audit logging with HMAC-sealed entries, database backups, and
  incident response.

~1.9 MB across Python, JavaScript, PHP and shell. 443 backend tests plus offline
rendering tests for the WordPress plugins that run in CI without a WordPress
install.

---

## seating-arrangements — constraint-based seating planner

A seating planner that assigns guests to tables under constraints — who must sit
together, who must be kept apart, table capacities — solved by a dedicated
solver rather than by hand.

**Stack:** Python 3.10 · FastAPI · Pydantic 2 · SQLAlchemy 2 · Alembic · a web
frontend

**Highlights:** a `core/solver` that turns the seating rules into an assignment
and a service layer over it, import/export of plans, a typed REST API with
OpenAPI docs, and database migrations. The interesting part is modelling the
rules cleanly and keeping the solver testable in isolation from the API.

---

## Canon-Quill — agent workflow for writing books

A conversation-first workflow that takes an author from reference material in
Google Drive to a finished, validated book exported as DOCX. You talk to a
`book-orchestrator` agent instead of memorising commands; it prepares the book,
writes each chapter in the chosen style, validates every chapter, builds the
final document and archives the project.

**Stack:** TypeScript · Model Context Protocol SDK · docx · Express · Zod · YAML

**Highlights:** an MCP server that bridges Google Drive, a staged pipeline
(prepare → write → validate → export → archive) with a validation gate on every
chapter, and DOCX generation. Schema-validated throughout with Zod.

---

## adventra-outreach — directory scraping and outreach

A local tool that syncs mountain-guide listings from the AEGM directory, tracks
each contact's state, and prepares personalised email batches — built for a real
outreach campaign.

**Stack:** Node.js · Express · SQLite · Cheerio · Nodemailer

**Highlights:** paginated scraping with on-demand enrichment of individual
profiles, a contact state machine (pending → contacted → replied → follow-up →
do-not-contact) with manual notes, and email sending that defaults to a dry run
so a misconfiguration never sends real mail. Supports SMTP or a Gmail app
password.

---

## How I work

- Tests and CI on everything that ships. A red pipeline blocks the deploy.
- Migrations are reviewed and reversible; production data gets backed up before
  destructive work.
- I measure before I optimise, and I report what the numbers actually say.

---

*Contribution activity here includes private repositories, so the graph reflects
real daily work even though the code is not public.*
