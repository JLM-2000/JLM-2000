# Javier Lucía Marco

Backend and full-stack engineer. I build and run production systems end to end —
API, workers, database, deployment pipeline and the client that talks to them.

The repositories below are private because they are commercial products in use.
This page describes what they do, how they are built and what I was responsible
for. Happy to walk through any of them in detail, or share code under NDA.

📍 Zaragoza, Spain · [LinkedIn](https://www.linkedin.com/in/) · j.luciamarco@gmail.com

---

## iFlySEO — AI content platform for WordPress

**Live product.** A multi-tenant SaaS that analyses a customer's website, infers
their service lines, and generates and publishes SEO content on a schedule.
Sold through a WooCommerce storefront with per-domain word accounting.

**Stack:** Python 3.11 · FastAPI · SQLAlchemy 2 (async) · Alembic · Celery ·
Redis · PostgreSQL 15 · Docker Compose · GitHub Actions · PHP 8 (WordPress
plugins) · vanilla JS

**What I built and run:**

- **Async API and worker fleet.** FastAPI service plus four Celery queues
  (generation, scraping, publishing, scheduling) behind Redis, with leased
  tasks, fenced completion and heartbeats so a dead worker cannot strand a job.
- **Content generation pipeline.** LLM generation with a validation layer —
  hard and soft constraints, candidate scoring, bounded retries — and an
  auditable record of every paid provider response. Reduced wasted paid retries
  from 8 per four articles to 2 by measuring the failures rather than guessing.
- **Two WordPress plugins in PHP.** A customer-site plugin (generation,
  scheduling, publishing, signed auto-update) and a Hub plugin that runs the
  storefront, customer portal and WooCommerce provisioning bridge.
- **Signed API boundary.** HMAC-SHA256 request signing with timestamp, nonce and
  body digest; encrypted secret storage; per-key rate limiting and lockout.
- **Deployment pipeline.** GitHub Actions: format, tests, PHP linting against
  each plugin's declared version, Semgrep, immutable image tags, health-gated
  deploy with automatic rollback markers, and semantic versioning derived from
  commit type.
- **Operational ownership.** VPS on Docker Compose with Prometheus and Grafana,
  tamper-evident audit logging with HMAC-sealed entries, database backups and
  incident response.

**Scale of the codebase:** ~1.9 MB across Python, JavaScript, PHP and shell.
443 backend tests plus offline rendering tests for the WordPress plugins that
run in CI without a WordPress install.

---

## canon-quill

**Stack:** TypeScript (96%) · shell

<!-- Describe in two or three lines: what it does, who it is for, what was hard
     about it, and what you owned. Recruiters read this section, not the code. -->

---

## adventra-outreach

**Stack:** JavaScript · CSS · HTML

<!-- Same: what it does, the interesting technical decision, your role. -->

---

## seating-arrangements

**Stack:** JavaScript · Python · CSS · HTML

<!-- Same. A constraint-solving or optimisation angle, if that is what it is,
     is worth spelling out — that reads well. -->

---

## How I work

- Tests and CI on everything that ships. A red pipeline blocks the deploy.
- Migrations are reviewed and reversible; production data gets backed up before
  destructive work.
- I measure before I optimise, and I report what the numbers actually say.

---

*Contribution activity on this profile includes private repositories, so the
graph reflects real daily work even though the code is not public.*
