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

## SeatWise — constraint-based seating optimiser

Assigns guests to tables under real constraints — who must sit together, who
must be kept apart, table capacities — and returns the best arrangement it can
find within a time budget, rather than leaving it to trial and error by hand.

**Stack:** Python 3.10 · FastAPI · Pydantic 2 · SQLAlchemy 2 · Alembic ·
server-rendered frontend

**How it works:**

- **Weighted scoring.** Every arrangement is scored with a breakdown; hard
  constraints carry penalties orders of magnitude larger than soft ones, so the
  optimiser never trades a "must not sit together" for a nicety.
- **Search.** Bounded hill climbing with random restarts and move/swap
  mutations across tables, plus a per-table ordering pass, all under a wall-clock
  deadline so the API always answers in time.
- **Grouping.** A union-find pass resolves "must sit together" chains into
  components before placement, with seat-distance fallbacks when a table cannot
  hold a whole group.
- Typed REST API with OpenAPI docs, import/export of plans, and database
  migrations. The solver is deliberately isolated from the API so it can be
  tested on its own.

---

## Canon-Quill — agent workflow for writing books

A conversation-first workflow that takes an author from reference material in
Google Drive to a finished, validated book exported as DOCX. You talk to an
orchestrator agent instead of memorising commands; it prepares the book, writes
each chapter in the chosen style, validates every chapter, builds the final
document and archives the project.

**Stack:** TypeScript · Model Context Protocol SDK · Google Drive API (OAuth) ·
docx · Express · Zod · YAML

**How it works:**

- **MCP Drive server.** A Model Context Protocol server bridges Google Drive so
  the agent reads reference material directly, with its own OAuth client.
- **Staged pipeline.** prepare → write → validate → export → archive, with a
  validation gate on every chapter before it can proceed, and a style check
  driven by a configurable list of AI clichés to avoid.
- **DOCX generation and a live markdown preview server** so the author sees the
  book take shape.
- A setup wizard, structured logging, and Zod schemas throughout.

---

## Adventra-Outreach — directory scraping and outreach

A local tool that syncs mountain-guide listings from the AEGM directory, tracks
each contact's state, and prepares personalised email batches — built for a real
outreach campaign.

**Stack:** Node.js · Express · SQLite · Cheerio · Nodemailer

**How it works:**

- **Paginated scraping** of the AEGM guide directory with on-demand enrichment
  of individual profiles to pull email and phone where published.
- **A contact state machine** — pending → contacted → replied → follow-up →
  do-not-contact — with manual notes, persisted in SQLite.
- **Safe sending.** Email defaults to a dry run, so a misconfiguration never
  sends real mail; supports SMTP or a Gmail app password.

---

## How I work

- Tests and CI on everything that ships. A red pipeline blocks the deploy.
- Migrations are reviewed and reversible; production data gets backed up before
  destructive work.
- I measure before I optimise, and I report what the numbers actually say.

---

*Contribution activity here includes private repositories, so the graph reflects
real daily work even though the code is not public.*
