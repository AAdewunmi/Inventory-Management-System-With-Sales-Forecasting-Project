# StockPulse: Inventory Management System & Sales Forecasting Platform

[![Status: Work in Progress](https://shields.io)](#project-status--roadmap)
[![CI Pipeline](https://shields.io)](#local-development--verification)
[![Code Style: Ruff](https://shields.io)](https://github.com)
[![Python](https://shields.io)](https://python.org)
[![Django](https://shields.io)](https://djangoproject.com)
[![PostgreSQL](https://shields.io)](https://postgresql.org)
[![Docker](https://shields.io)](https://docker.com)
[![License: MIT](https://shields.io)](LICENSE)

> **Note:** StockPulse is currently under active development. Core domain models, database schemas, and initial workflow views are being built iteratively. Features and interfaces are subject to change until `v1.0.0`.

StockPulse is a full-stack web application built for small businesses to track real-time inventory, process concurrency-safe point-of-sale transactions, and use historical sales data to forecast future demand. It pairs an interactive HTMX and ApexCharts interface with a Django 5, PostgreSQL, Celery, and `statsmodels` backend.

---

## Project Status & Roadmap

This repository is an active work in progress. Development is structured into five incremental milestones:

* [x] **Milestone 1: Foundations & Domain Schema** — Repository setup, Docker Compose orchestration, modular Django settings (`local`/`test`/`production`), Ruff/Black quality gates, and core inventory/ledger schema design.
* [ ] **Milestone 2: Auth, RBAC & Catalog Management** — Custom email-first user model, Admin vs. Staff role permissions, and CRUD workflows for products, suppliers, and stock levels. *(In Progress)*
* [ ] **Milestone 3: Concurrency-Safe POS & Audit Ledger** — HTMX live Point-of-Sale checkout screen using `transaction.atomic()` and `select_for_update()` row locking, paired with an immutable `StockMovement` ledger.
* [ ] **Milestone 4: Time-Series Forecasting & Smart Reorders** — Weekly Celery Beat forecasting pipeline (`statsmodels` SARIMA / Exponential Smoothing), ApexCharts 30-day demand curves, and one-click PDF Purchase Order generation.
* [ ] **Milestone 5: Testing, Demo Seeding & Deployment** — Comprehensive `pytest-django` suite (`>=85%` coverage) including concurrency tests, 12-month seasonal demo seed command, Sentry monitoring, and cloud deployment.

---

## Planned Core Capabilities

* **Full-Stack Operations & POS UI:** Reactive HTMX Point-of-Sale interface with debounced SKU search, barcode scanner input support, role-based dashboards, and interactive ApexCharts demand curves.
* **Data Integrity & Forecasting:** Concurrency-safe inventory deductions preventing negative stock race conditions, an immutable stock movement audit trail, automated 30-day SKU demand projections, and smart reorder alerts.

---

## Tech Stack & Architecture

* **Backend & API:** Python 3.12+, Django 5.x, Django REST Framework
* **Frontend:** Django Templates, Tailwind CSS, HTMX, Alpine.js, ApexCharts
* **Forecasting & Workers:** Pandas, NumPy, `statsmodels`, Celery, Celery Beat, Redis 7
* **Database & Ops:** PostgreSQL 16, Docker, Docker Compose, Gunicorn, WhiteNoise, Nginx, WeasyPrint, GitHub Actions

```text
[Browser (HTMX + ApexCharts)] ──► [Nginx] ──► [Django 5 + Gunicorn] ──► [PostgreSQL 16]
                                                        │
                                                        ▼
     [Celery Beat] ──► [Celery Workers] ──► [statsmodels Forecasting & Low-Stock Alerts]
```

---

## Quick Start (Development Environment)

1. **Clone and Configure:**
   ```bash
   git clone https://github.com
   cd StockPulse-Inventory-Forecasting
   cp .env.example .env
   ```
2. **Build and Start Services:**
   ```bash
   make build && make up && make migrate && make seed
   ```
   * **Web App:** `http://localhost:8000` | **Admin Console:** `http://localhost:8000/admin/`

### Planned Demo Accounts *(Available upon Milestone 5 completion)*
* **Admin Account:** `admin.demo@stockpulse.local` / `StockPulseDemoPass123`
* **Staff POS Account:** `staff.demo@stockpulse.local` / `StockPulseDemoPass123`

---

## Local Development & Verification

Standard development and quality-gate commands are available via the `Makefile`:
* `make up` / `make logs` / `make migrate` / `make seed`
* `make test` (`pytest-django`), `make test-cov` (`>=85%` coverage gate), `make lint` (Ruff), `make format-check` (Black), and `make check` (full quality gate).

The repository structure cleanly separates domain apps (`apps/users/`, `apps/inventory/`, `apps/sales/`, `apps/forecasting/`, `apps/dashboard/`), environment settings (`config/settings/`), and automated GitHub Actions CI pipelines (`.github/workflows/ci.yml`).

---

## License

Licensed under the MIT License. See [LICENSE](LICENSE) for details.
