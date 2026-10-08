# NOVA

NOVA is a Persian-first Telegram assistant for channel publishing, content workflows, subscriptions and account support

## Live bot

- Telegram bot: [@NovaPls_bot](https://t.me/NovaPls_bot)
- Public Telegram bot ID: **8881657128**
- Main interface language: Persian with right-to-left support

## Interface previews

These pictures show design concepts for the NOVA web workspace and checkout
They are illustrative previews and are not screenshots proving that every screen is live

![Supernova workspace design preview for NOVA](https://github.com/ahurkkkkkkk/NOVA/releases/download/readme-previews/NOVA-option-3-supernova-workspace.png)

![Accessible plan and checkout component states](https://github.com/ahurkkkkkkk/NOVA/releases/download/readme-previews/NOVA-PlanCheckout.png)

![Polaris storefront design preview](https://github.com/ahurkkkkkkk/NOVA/releases/download/readme-previews/NOVA-Polaris-ui-preview.png)

## What customers can do in Telegram

- Start the bot, complete onboarding and use NOVA features allowed by their account and subscription
- Browse administrator-configured plans and review available durations, capabilities and usage limits
- Pay for Telegram digital services with Telegram Stars when that method is enabled
- Submit a bank transfer receipt for review when that payment method is enabled
- View account access, subscription status, limits, payment history and support context
- Pause or resume their own NOVA connections where that control is available
- Connect source channels and destinations for channel-to-channel mirroring
- Create automatic publishing feeds from the source catalogue or supported web, RSS and API sources
- Set delivery intervals, item limits, text length, translation, summary and writing tone
- Set include and exclude terms, advertising signals, urgency rules, source attribution and a custom footer
- Configure comment assistance for a linked discussion, including a prompt, tone and saved channel rules
- Build channel context from history and ask questions using channel memory
- Submit text, links or supported media for one-off processing, translation or summarization
- Use an invite link, review referral activity and request eligible withdrawals through the referral wallet
- Open a support request and follow payment or account issues

## What bot administrators can manage

- Customer access, user status, subscriptions, receipts and payment history
- Plans, plan features, limits, prices and trial settings from the protected bot admin controls
- AI provider connections, model choices, provider priority, fallback and configured source keys
- Feed sources, custom source review, channel connections and destination requirements
- Required membership, channel-admin requirements and global publishing controls
- Bot text, button labels, presentation settings, support details and broadcasts
- Support requests, targeted customer messages, referral contracts and withdrawal requests
- Operational status, audit information and protected backups

Plan and trial values are intended to come from administrator settings rather than fixed storefront copy
The web workspace source also contains plan and pricing controls, but simultaneous updates from both admin panels have not been independently verified end to end

## Web workspace and team backend

The NOVA Python backend also contains workspace APIs for organizations and members, channel setup, brand and writing rules, source research, content verification, drafting, media, approvals, scheduled publishing, channel strategy, analytics, growth, advertiser campaigns and organization billing

Those API capabilities are separate from the Telegram bot menus
An API route in the source does not by itself prove that its web screen is complete, connected or validated in production

## Backend technologies and techniques

This inventory describes patterns found in the audited live Python source and its deployment configuration
The source review counted 117 HTTP routes across the API
It records implementation evidence and does not claim that every route has a finished or production-validated screen

### Application architecture

- Python 3.12 with FastAPI and Starlette for an asynchronous ASGI application served by Uvicorn
- Versioned REST routes with Pydantic request and response schemas, validation and consistent exception handling
- Telegram bot and channel integrations implemented with Telethon
- Modular router, service and repository layers with dependency-injected database sessions
- A modular backend process with separate Telegram bot, database, Redis and migration services in the Python deployment configuration
- Explicit content and publishing state machines with approval gates, tenant ownership checks and role-based permissions
- The Persian web workspace is served from the backend on the same site as its API

### Data, transactions and jobs

- PostgreSQL 16 for production data with SQLAlchemy 2 asynchronous sessions and the asyncpg driver
- Alembic schema migrations run as a separate deployment step
- SQLite support for isolated development and tests
- Request-scoped database transactions that commit after success and roll back on exceptions, with savepoints for selected conflict handling
- Redis 7 for shared rate limits, cached values, login challenges, sessions, distributed locks and cross-process job queues
- A Redis-backed queue pattern with claims, acknowledgements, in-flight recovery, retries, dead-letter storage and operator replay
- An asynchronous Python scheduler implementation with time-zone-aware jobs, retry and backoff behavior
- A durable notification outbox with claimed rows, retry controls and encrypted message payloads
- Idempotency keys for publishing requests and SHA-256 fingerprints for canonical content and source URLs

### Integrations and content processing

- Asynchronous HTTP integrations through HTTPX and aiohttp with request timeouts
- Provider adapters for OpenAI-compatible endpoints and Gemini, configurable model selection, provider priority and fallback
- Transient AI error retries with exponential backoff, normalized provider errors, usage records and a mock provider for tests
- A staged pipeline for research, duplicate and quality checks, verification, writing, media, approval and publication
- Deterministic advertisement heuristics alongside AI-assisted verification and content generation guardrails
- Source URL validation that restricts protocols and ports, rejects embedded credentials and private or reserved network targets, and disables redirects in collectors
- Upload validation using file signatures, declared MIME checks, size limits and sanitized filenames
- Local and S3-compatible media storage adapters

### Identity, authorization and protection

- Password storage using salted bcrypt with a SHA-256 pre-hash for new passwords and compatibility checks for older hashes
- Signed, typed JWTs for access, refresh and password-reset flows
- Telegram Web App identity verification using Telegram HMAC data validation
- A short-lived Telegram login challenge held in Redis, bound to the approving Telegram user and browser challenge
- HttpOnly, Secure and SameSite cookies, origin checks and CSRF token validation for browser sessions
- Role-based access checks and organization-scoped ownership checks on protected resources
- API rate limits separated by authentication, administrator, payment and general request categories
- Request body limits, CORS origin controls, security response headers and trusted-proxy IP handling
- Production startup validation that rejects missing or known-weak critical secrets
- Versioned application-level OpenSSL AES-256-CBC encryption with PBKDF2 and HMAC authentication for selected bot secrets and notification payloads, with key identifiers and a key-rotation routine
- Backup encryption using OpenSSL AES-256-CBC with PBKDF2 and a separate HMAC-SHA256 integrity check
- PostgreSQL and SQLite backup verification, dump checksums and guarded restore workflows
- Tamper-evident audit records linked into scoped HMAC-SHA256 hash chains, with signed checkpoints and optional S3 Object Lock storage
- Sensitive-field masking in audit and administrative output

### Operations and engineering checks

- Docker Compose services with persistent data volumes, database health checks, separate schema migration, and bounded container resources
- Read-only container filesystems, dropped Linux capabilities and no-new-privileges settings on the Python application services
- JSON structured logs with request IDs for tracing an API action through its logs and audit entries
- Health and readiness endpoints, service heartbeat files, and Prometheus-format counters and gauges
- Pytest and pytest-asyncio configuration for unit, integration, end-to-end and external-service tests
- Ruff linting, strict mypy configuration, branch coverage, Bandit and pip-audit tooling configured in the Python project

The production deployment also lists Go scheduler and worker containers, but only their binaries were available during the source review
The Python scheduler and queue descriptions above refer to Python implementations found in source and should not be read as a review of those Go binaries

## Important limits

- Duplicate screening and channel memory code exist, but they do not guarantee that every repeated story will be stopped
- Uncertain duplicate or suspicious-post decisions are not yet verified as a private approval queue for the administrator of each destination channel
- Trial settings exist in bot administration, but web-only trial activation has not been verified in the audited bot source
- Crypto checkout is not connected to a payment provider and is not available as a completed purchase flow
- The deployed Go scheduler and worker are binaries whose source was not available for review, so their internal behavior is not documented here
- Some web workspace routes and administrative sections still need end-to-end validation

This description is based on the verified live Python source and is careful to separate implemented code from incomplete or unverified behavior

## Public repository

This repository keeps only the README in its tracked source tree
The design preview pictures are attached to the [README previews release](https://github.com/ahurkkkkkkk/NOVA/releases/tag/readme-previews)
No bot source code, web app source code, credentials, customer data or runtime configuration is published here
