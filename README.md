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

Plan prices and trial length are stored as administrator settings rather than fixed storefront copy
The bot admin and authenticated web admin source use the same database-backed plan catalogue with revision checks, audit records and conflict responses for stale edits, while simultaneous production use of both panels has not been independently exercised

## Web workspace and team backend

The NOVA Python backend also contains workspace APIs for organizations and members, channel setup, brand and writing rules, source research, content verification, drafting, media, approvals, scheduled publishing, channel strategy, analytics, growth, advertiser campaigns and organization billing

Those API capabilities are separate from the Telegram bot menus
An API route in the source does not by itself prove that its web screen is complete, connected or validated in production

## Backend technologies and techniques

This inventory describes patterns found in the audited live Python source and its deployment configuration
The Python API is organized into versioned routers and service modules, with implementation details mapped below
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

## How the application is built

The audited Python source is organized as an asynchronous FastAPI service, SQLAlchemy data models and repositories, Telegram handlers built with Telethon, and smaller service modules for AI providers, sources, publishing, billing, moderation, analytics and the customer workspace

The storefront HTML, CSS, JavaScript and static Polaris logo are packaged with the backend under `app/modules/storefront/web`, while API routes validate requests and delegate the work to service and database layers
The reviewed webapp uses a static bundled Polaris logo and has no runtime animation switch

External network calls are commonly isolated behind injectable fetchers or provider adapters so tests can substitute deterministic responses, database changes are represented as ordered Alembic migrations, and the Python test suite uses pytest

The live source snapshot had no Git metadata, so its exact author history, who wrote each file, the original prompts and the development sequence cannot be recovered from that copy
The script index below describes current file purpose from module documentation, migration names, test names and top-level symbols, not undocumented authorship history

### Main tool paths

- `app/modules/research` discovers and normalizes content from search and configured sources, while `app/modules/research/collectors` implements reusable collectors
- `app/modules/telegram` connects Telegram commands, channel mirroring, AI chat, account support, administrator flows, content checks and channel memory
- `app/modules/ai` routes requests through configurable external AI provider adapters and normalizes their output and failures
- `app/modules/content_pipeline`, `app/modules/writer`, `app/modules/verification` and `app/modules/approvals` implement research, drafting, verification and human approval steps
- `app/modules/publishing`, `app/modules/channels` and `app/modules/media` handle destinations, publishing state, uploaded media and local or S3-compatible storage
- `app/modules/storefront`, `app/modules/billing` and `app/modules/monetization` provide plan catalogues, subscription records, Telegram Stars and bank transfer workflows
- `app/modules/organizations`, `app/modules/strategy`, `app/modules/growth`, `app/modules/analytics`, `app/modules/community` and related modules provide the larger team workspace API, which does not prove that every related screen is finished or enabled in production
- Root scripts and `scripts/` include deployment, health, migration and operator helpers, while `tests/` holds regression and service tests

## Scrapers and live-news freshness

All source collectors follow a shared pattern of validating the configured source, fetching it with timeout, retry, backoff and rate policy, parsing it into a normalized entry, deduplicating it and saving it
The network functions are injectable for offline tests

- Google News RSS builds a query with `when:1d`, requests Persian and Iran defaults when the source does not override them, parses the feed with the shared RSS parser and rejects entries without a parseable published timestamp, older than 24 hours or more than five minutes in the future
- RSS and Atom feeds are parsed with Python's standard XML parser into a common title, text, URL, source ID and publication-time record
- Website pages use Python's HTML parser to extract readable blocks and skip script, style and other non-content elements
- REST API sources fetch JSON and support an item path, field mapping and an optional API key in a header or query parameter
- Telegram public-channel collection reads the public `t.me/s/<channel>` preview and extracts message text and IDs, while private invite-only channels are unsupported by that collector
- Manual sources accept operator-provided text or entries, and custom sources can be registered behind a shared collector interface
- Shared URL checks reject unsupported schemes, embedded credentials, nonstandard ports and private or reserved network addresses, while bounded-response helpers limit collector response size and the normal HTTP collectors disable redirects
- `fact_checker.py` contains multi-search adapters for DuckDuckGo, Google News, Bing and SearXNG, plus timestamp, source reputation and topic relevance checks
- `sources_catalog.py` contains a separate source catalogue and provider-specific metadata including optional thumbnail URLs, which is not the same thing as semantic image selection for an article

The source filters Google News timestamps in code, but upstream timestamps can be wrong, other providers can return stale results, and this source audit did not measure freshness or delivery latency on the running service

## Persistent memory and duplicate screening

NOVA stores channel publication memory in the database rather than updating model weights
After a successful channel publication, the bot creates the memory row and then asks the configured language model to compress the text into a topic, facts, entities, keywords and related context
The original text remains as a fallback if compression fails

Channel recall loads recent records for that channel, ranks them with token overlap and saved keywords or topics, and adds the best matching facts to an AI prompt
The code also has a bounded backfill path for recent Telegram posts
This is persistent contextual memory with lexical retrieval, not a vector database

Before sending a feed or mirror item, deterministic recent-story checks compare its source item ID, URL and story text
For longer posts, the bot reads up to 500 memories for the exact destination channel, uses token overlap and Jaccard similarity to shortlist candidates, then asks an external model whether the candidate describes the same concrete event
A match is suppressed only when the model returns a known memory ID and confidence of at least 0.88, a confident new result needs at least 0.80, and uncertain or failed AI checks are held for review
This layered check lowers repeats but cannot guarantee that every repeat will be identified

`comment_learner.py` asks a language model to turn explicit feedback into structured rules such as tone, FAQ, prohibition and writing guidance, then saves and retrieves those records by lexical relevance to condition future prompts
`user_memory.py` keeps limited user-facing profile and interaction context for personalization
These are database-backed prompt memories and rule retrieval, not fine-tuning

## Channel ID whitelist and channel-admin review

When a destination is resolved and verified, the bot stores its exact Telegram channel ID in the destination whitelist
A destination is eligible only when that owner's channel ID is active in the whitelist

Possible advertisements, stale items and uncertain duplicates can be saved to a per-channel publication review queue instead of being published immediately
A channel administrator opens a private chat with the bot and uses `/review` or `/pending` to see pending items for channels where Telegram currently reports that person as an administrator
`/whitelist` shows verified destination IDs and pending counts only for channels that person currently administers

The approve and reject buttons recheck that the person is an administrator of that exact destination, reject duplicate decisions safely, and on approval revalidate that the feed or mirror is still active and points to the same channel before publishing
A global NOVA bot administrator does not get a bypass for another channel's review
If the bot cannot start a private conversation, an authorized channel admin can still retrieve pending items using `/review`

The Python source includes the whitelist and review tables in migration `0053_channel_whitelist_review`, and focused tests cover destination-admin scoping, duplicates, ambiguous decisions and review behavior
The deployed database migration, actual Telegram permissions and end-to-end callback delivery were not exercised in this documentation pass

## Advertising, quality and content filters

`app/modules/smart_filters.py` normalizes multilingual text including Persian and Arabic character variants, removes diacritics and punctuation, then checks configured include and exclude terms with exact and fuzzy matching
An optional external Gemini embeddings backend can compare semantic similarity with cosine distance, but it is disabled by default unless configured
The filter can use a language model for uncertain cases and retains deterministic decisions if that service fails

`app/modules/telegram/content_gate.py` combines deterministic link, call-to-action, urgency and advertisement signals with an optional structured AI judgement for ad status, importance, urgency and misinformation risk
Its settings are administrator-configurable and the feed and mirror publishing handlers can route blocked or suspicious cases into the per-channel review queue

The fact checker combines search results with publication-time, source-reputation and topic relevance checks, while caches reduce repeated lookups
These checks help sort and flag content, but they cannot establish that every source is truthful or that every advertisement will be detected

## ML, vision, images and fine-tuning

The inspected Python project uses external language-model inference and optional external text embeddings, plus ordinary deterministic Python matching and scoring
The source and dependencies do not show an in-house trained machine-learning model, a local model-weight package, an ML training pipeline or a fine-tuning dataset, training job, evaluation run or checkpoint
The code calls hosted models with prompts and structured-output instructions, which is inference and prompt conditioning rather than fine-tuning

The audited photo path handles media transport and formatting
Pillow corrects image orientation, converts supported modes to RGB and makes an optimized JPEG, while media validation checks file signatures, declared MIME type, size and safe filenames
Telegram photos and videos have bounded download paths, and remote article images are downloaded only from validated public URLs
The publishing layer can attach a configured `image_url`, and the source catalogue can carry thumbnail metadata

No image-to-model payload, vision classification pipeline, OCR tool or visual article-to-photo matching implementation was found in the inspected Python modules
That means the source proves how supported images are carried and resized, not that NOVA understands an image or chooses a semantically correct picture
The Go scheduler and worker source was unavailable, so this statement applies to the reviewed Python source only

## Complete script-by-script inventory

The audited source snapshot contains 494 Python files, 47 shell scripts, 2 PowerShell scripts and 2 JavaScript files, excluding compiled Python caches
The list below includes application modules, tests, database revisions, deployment helpers and frontend scripts
Python purpose notes use module docstrings or top-level names, migration notes use the revision description, test notes use test names, and shell or PowerShell purpose is identified by its filename

<details>
<summary>Open the full 545-file inventory</summary>


**.**
- `activate_user_trial.py`: Defines main
- `admins.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `auth_userbot.py`: One-time userbot authentication script
- `bot_users.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check10.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check11.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check2.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check3.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check4.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check5.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check6.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check7.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check8.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check9.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check_admins.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check_nova_ai_db.py`: Defines main
- `check_phone.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check_session.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `check_settings.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `comment_learner.py`: Comment Machine Learning and Semantic Memory for Nova
- `count_users.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `deploy_rtl_creds.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `final.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `final2.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `final_check.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `fix-migrate.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `fix2.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `health2.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `humanizer.py`: Global Humanizer &amp; Anti-AI Content Transformer for Nova
- `inspect_ahu.py`: Defines main
- `inspect_plans.py`: Defines main
- `inspect_recent.py`: Defines main
- `install-nova.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `mlog.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `Nova.ps1`: PowerShell launcher or operator helper; exact purpose is indicated by the filename
- `patch_bot_extensions.py`: Python package marker or module constants
- `poll_bc.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `post_activate.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `rebuild.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `restart.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `restart_after_setting.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `restart_bot2.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `rtl_verify.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `run_bot.py`: Nova Telegram Bot runner
- `run_scheduler.py`: NOVA scheduler entry point
- `run_worker.py`: Durable NOVA worker process backed by the shared Redis queue
- `send_code.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `send_code2.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `send_code3.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `send_code4.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `set_trial.py`: Defines main
- `set_userbot_admin.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `sign_in.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `signin2.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `signin3.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `START_NOVA.ps1`: PowerShell launcher or operator helper; exact purpose is indicated by the filename
- `stats.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `test_blockquote.py`: Python package marker or module constants
- `test_jev.py`: Defines main
- `test_suite_vps.py`: Defines MockApp, test_ad_detection, test_political_news_pass, test_story_tokens_and_deduplication

**app**
- `app/__init__.py`: NOVA backend application package
- `app/main.py`: FastAPI application entrypoint for NOVA

**app/api**
- `app/api/__init__.py`: Python package marker or module constants
- `app/api/deps.py`: Shared API dependencies
- `app/api/exception_handlers.py`: Global exception handlers producing the standard error envelope
- `app/api/health.py`: Liveness and readiness health endpoints (Stage 29)
- `app/api/middleware.py`: Request-context middleware: request id + structured access logging
- `app/api/request_limits.py`: Dependency-light ASGI request-body limits for every HTTP endpoint
- `app/api/router.py`: Top-level versioned API router
- `app/api/security_middleware.py`: Security middleware for Stage 28

**app/api/routes**
- `app/api/routes/__init__.py`: Python package marker or module constants
- `app/api/routes/health.py`: Health check endpoints
- `app/api/routes/monitoring.py`: Operational monitoring endpoints

**app/core**
- `app/core/__init__.py`: Python package marker or module constants
- `app/core/audit_chain.py`: HMAC hash-chain primitives for tamper-evident audit persistence
- `app/core/audit_checkpoint.py`: Signed external checkpoints that make audit tail deletion detectable
- `app/core/audit_checkpoint_store.py`: Configuration factory for external audit-checkpoint storage
- `app/core/audit_checkpoint_worm.py`: Dependency-light S3 Object Lock backend for audit checkpoints
- `app/core/backup_integrity.py`: Native database dump integrity checks independent of application state
- `app/core/bounded_text.py`: Deterministically fit untrusted text into bounded database columns
- `app/core/cache.py`: Standard cache layer: TTL, namespacing, invalidation, and metrics
- `app/core/config.py`: Environment-based application settings (Pydantic v2)
- `app/core/email.py`: Small SMTP adapter used for security-sensitive transactional email
- `app/core/env_validation.py`: Environment validation (Stage 30)
- `app/core/errors.py`: Structured application errors and the standard API error envelope
- `app/core/heartbeat.py`: Cross-process heartbeat inspection used by operational monitoring
- `app/core/logging.py`: Structured logging: console (JSON or plain), rotating file, request ids
- `app/core/metrics.py`: In-process metrics registry with Prometheus text exposition
- `app/core/pagination.py`: Reusable pagination utilities (Stage 29)
- `app/core/rbac.py`: Centralized, reusable role-based access control (RBAC)
- `app/core/redis.py`: Redis facade: pooled client, rate limiting, locks, sessions, queues
- `app/core/request_id.py`: Bounded request-id normalization safe for logs and response headers
- `app/core/security.py`: Security primitives: password hashing and JWT handling
- `app/core/security_utils.py`: Centralized security utilities

**app/db**
- `app/db/__init__.py`: Database infrastructure layer
- `app/db/base.py`: Declarative base for all ORM models
- `app/db/bootstrap.py`: Database bootstrap policy shared by every entry point
- `app/db/dev_schema.py`: Reusable SQLite development schema bootstrap
- `app/db/session.py`: Async database engine and session management

**app/models**
- `app/models/__init__.py`: ORM models package
- `app/models/advertiser.py`: Advertiser model - an organization-scoped ad partner
- `app/models/ai_usage.py`: AI usage logging model
- `app/models/analytics_metric.py`: AnalyticsMetric model - a single raw recorded metric point
- `app/models/approval_request.py`: ApprovalRequest model - a generic, auditable approval record
- `app/models/audit_log.py`: Production audit log model - general entity-change history
- `app/models/billing_invoice.py`: Billing invoice model
- `app/models/billing_payment.py`: Billing payment model
- `app/models/bot_extensions.py`: Telegram-only product models: subscriptions, mirroring, comments, receipts,
- `app/models/calendar_slot.py`: CalendarSlot model - a single scheduled content slot within a strategy plan
- `app/models/campaign.py`: Campaign model - an advertising campaign on a channel
- `app/models/channel.py`: Channel model - multi-channel management core
- `app/models/channel_growth_snapshot.py`: ChannelGrowthSnapshot - aggregated channel growth over a period
- `app/models/community_message.py`: CommunityMessage model - one analyzed channel comment/message
- `app/models/content_item.py`: ContentItem model - a unit of content moving through the pipeline
- `app/models/content_performance_snapshot.py`: ContentPerformanceSnapshot - aggregated content performance over a period
- `app/models/content_source.py`: ContentSource model - channel-scoped content source configuration
- `app/models/decision_log.py`: Decision log model for Nova Core safety decisions
- `app/models/digital_twin.py`: Digital Twin model
- `app/models/growth_report.py`: GrowthReport model - one stored, immutable growth report
- `app/models/media_asset.py`: MediaAsset model - metadata for an uploaded media file
- `app/models/notification_outbox.py`: Durable transactional outbox for external notifications
- `app/models/organization.py`: Organization and membership models (multi-tenancy foundation)
- `app/models/organization_member.py`: Backward-compatible import for OrganizationMember
- `app/models/plan.py`: Plan model - a configurable SaaS subscription plan
- `app/models/publishing_job.py`: PublishingJob model - a scheduled or immediate Telegram publish request
- `app/models/referral.py`: Telegram referral network used for lawful, transparent reward credits
- `app/models/revenue_record.py`: RevenueRecord model - a tracked (not processed) revenue entry
- `app/models/strategy_plan.py`: StrategyPlan model - a single versioned content strategy plan
- `app/models/subscription.py`: Subscription model - links an organization to a plan
- `app/models/usage_counter.py`: UsageCounter model - per-organization, per-day usage counting
- `app/models/user.py`: User model - authentication and identity foundation
- `app/models/web_workspace.py`: Durable web-to-bot operations and support conversations

**app/modules**
- `app/modules/__init__.py`: Business modules package
- `app/modules/key_rotation.py`: One-transaction rotation for every encrypted-at-rest database payload
- `app/modules/smart_filters.py`: Hybrid multilingual semantic tunnel/exclude filters

**app/modules/admin**
- `app/modules/admin/__init__.py`: System admin module (Stage 27)
- `app/modules/admin/router.py`: System admin router - every endpoint is strictly superuser-only
- `app/modules/admin/schemas.py`: Admin schemas. Sensitive fields are masked or omitted by design
- `app/modules/admin/service.py`: AdminService - system-wide statistics and listings (superuser only)

**app/modules/ai**
- `app/modules/ai/__init__.py`: Provider-agnostic AI layer
- `app/modules/ai/auth.py`: Authentication helpers for OpenAI-compatible gateways
- `app/modules/ai/errors.py`: AI layer error hierarchy
- `app/modules/ai/pricing.py`: Cost estimation for AI usage
- `app/modules/ai/registry.py`: Provider registry - the swap point for AI providers
- `app/modules/ai/repository.py`: Database-backed usage sink for the AI service
- `app/modules/ai/retry.py`: Async retry helper for transient AI provider failures
- `app/modules/ai/schemas.py`: Structured request/response schemas and the plain usage record
- `app/modules/ai/service.py`: The single, clean AI text-generation service

**app/modules/ai/providers**
- `app/modules/ai/providers/__init__.py`: Swappable LLM providers behind a common interface
- `app/modules/ai/providers/base.py`: Base LLM provider interface
- `app/modules/ai/providers/mock.py`: Deterministic mock provider for local development and tests
- `app/modules/ai/providers/openai_compatible.py`: OpenAI-compatible chat-completions provider

**app/modules/analytics**
- `app/modules/analytics/__init__.py`: Analytics module - safe, deterministic metrics storage and aggregation
- `app/modules/analytics/aggregation.py`: Pure, deterministic aggregation functions - no DB/framework imports
- `app/modules/analytics/collectors.py`: Analytics collectors - isolated, mockable data collection
- `app/modules/analytics/enums.py`: Analytics enums - dependency-free for standalone testing
- `app/modules/analytics/errors.py`: Analytics module error hierarchy
- `app/modules/analytics/repository.py`: Analytics repository - data access layer
- `app/modules/analytics/router.py`: Analytics API router (organization-scoped, thin)
- `app/modules/analytics/schemas.py`: Pydantic schemas for the analytics module
- `app/modules/analytics/service.py`: AnalyticsService - collection, storage, and aggregation orchestration

**app/modules/approvals**
- `app/modules/approvals/__init__.py`: Approvals - Nova's generic, auditable approval workflow
- `app/modules/approvals/enums.py`: Approval vocabulary enums
- `app/modules/approvals/errors.py`: Typed errors for the approval workflow
- `app/modules/approvals/nova_core_link.py`: Explicit, testable bridge from a Nova Core decision to an approval request
- `app/modules/approvals/policy.py`: Pure approval policy: RBAC mapping, default risk, and executability
- `app/modules/approvals/repository.py`: Persistence for approval requests
- `app/modules/approvals/router.py`: Approvals HTTP router (organization-scoped, thin)
- `app/modules/approvals/schemas.py`: Pydantic schemas for approval input/output validation
- `app/modules/approvals/service.py`: Approval service - all approval business logic lives here

**app/modules/audit**
- `app/modules/audit/__init__.py`: Audit module - production-grade, organization-scoped audit logging
- `app/modules/audit/masking.py`: Sensitive-data masking for audit logs and logging
- `app/modules/audit/repository.py`: Persistence for audit logs
- `app/modules/audit/router.py`: Audit log query API (organization-scoped, read-only)
- `app/modules/audit/schemas.py`: Audit log schemas (input + API response)
- `app/modules/audit/service.py`: Audit service - the single, safe entry point for writing audit logs
- `app/modules/audit/verification.py`: Database adapter for full persisted audit-chain verification

**app/modules/billing**
- `app/modules/billing/__init__.py`: Billing module
- `app/modules/billing/enums.py`: Billing enums (dependency-free)
- `app/modules/billing/errors.py`: Typed errors for the billing module
- `app/modules/billing/plans_config.py`: Default plan catalog - the ONE place default plan behavior lives
- `app/modules/billing/repository.py`: Billing persistence (plans, subscriptions, usage counters, invoices, payments)
- `app/modules/billing/router.py`: Defines list_plans, create_plan, assign_subscription, get_subscription
- `app/modules/billing/schemas.py`: Billing schemas
- `app/modules/billing/service.py`: Billing service

**app/modules/channels**
- `app/modules/channels/__init__.py`: Channels module (service / repository / router)
- `app/modules/channels/repository.py`: Channel repository - the only place that talks to the database for channels
- `app/modules/channels/router.py`: Channels HTTP router (organization-scoped)
- `app/modules/channels/schemas.py`: Pydantic schemas for the channels module
- `app/modules/channels/service.py`: Channel service - business logic, settings, and status lifecycle

**app/modules/community**
- `app/modules/community/__init__.py`: Community Agent module (Stage 24)
- `app/modules/community/agent.py`: Community Agent - analyzes one comment into a validated analysis
- `app/modules/community/enums.py`: Community module enums (dependency-free)
- `app/modules/community/errors.py`: Typed errors for the community module
- `app/modules/community/generator.py`: Community analyzers
- `app/modules/community/parsing.py`: Parsing &amp; validation of the AI community-analysis response (pure)
- `app/modules/community/repository.py`: CommunityMessage persistence (thin data access, org-scoped)
- `app/modules/community/router.py`: Community HTTP router (organization- and channel-scoped)
- `app/modules/community/schemas.py`: Community schemas: validated AI analysis + API models
- `app/modules/community/service.py`: Community service - storage, privacy, and approval wiring (no AI here)

**app/modules/content_pipeline**
- `app/modules/content_pipeline/__init__.py`: Content pipeline - ContentItem storage and the processing state machine
- `app/modules/content_pipeline/hashing.py`: Deterministic content hashing for duplicate detection
- `app/modules/content_pipeline/repository.py`: Content item repository - the only place that queries content items
- `app/modules/content_pipeline/router.py`: Content pipeline HTTP router (organization- and channel-scoped)
- `app/modules/content_pipeline/schemas.py`: Pydantic schemas for the content pipeline module
- `app/modules/content_pipeline/service.py`: Content pipeline service - manual creation and controlled transitions
- `app/modules/content_pipeline/state_machine.py`: The content pipeline state machine (pure, dependency-light)

**app/modules/content_sources**
- `app/modules/content_sources/__init__.py`: Content source management
- `app/modules/content_sources/repository.py`: Content source repository - the only place that queries content sources
- `app/modules/content_sources/router.py`: Content sources HTTP router (organization- and channel-scoped)
- `app/modules/content_sources/schemas.py`: Pydantic schemas for the content sources module
- `app/modules/content_sources/service.py`: Content source service - channel-scoped, organization-isolated CRUD
- `app/modules/content_sources/validation.py`: Pure, dependency-light validation helpers for content sources

**app/modules/digital_twins**
- `app/modules/digital_twins/__init__.py`: Channel onboarding &amp; Digital Twin module (schemas/repo/service/router)
- `app/modules/digital_twins/repository.py`: Digital Twin repository - the only place that queries the digital_twins table
- `app/modules/digital_twins/router.py`: Digital Twin onboarding router (organization- and channel-scoped)
- `app/modules/digital_twins/schemas.py`: Validated Pydantic schemas for the Digital Twin onboarding profile
- `app/modules/digital_twins/service.py`: Digital Twin service - onboarding business logic

**app/modules/growth**
- `app/modules/growth/__init__.py`: Growth Agent module (Stage 23)
- `app/modules/growth/agent.py`: Growth Agent - extracts analytics data and produces a validated report
- `app/modules/growth/enums.py`: Growth report enums
- `app/modules/growth/errors.py`: Typed errors for the growth module
- `app/modules/growth/extraction.py`: Pure analytics-data extraction for growth reports (fully testable)
- `app/modules/growth/generator.py`: Growth report generators
- `app/modules/growth/parsing.py`: Parsing &amp; validation of the AI growth-report response (pure)
- `app/modules/growth/repository.py`: GrowthReport persistence (thin data access, org-scoped)
- `app/modules/growth/router.py`: Growth report HTTP router (organization- and channel-scoped)
- `app/modules/growth/schemas.py`: Growth report schemas: validated AI output + API read models
- `app/modules/growth/service.py`: Growth report service - storage + tenant checks (no AI here)
- `app/modules/growth/tasks.py`: Reusable scheduled growth-report batch entry point

**app/modules/media**
- `app/modules/media/__init__.py`: Media module - Nova's basic media management system
- `app/modules/media/errors.py`: Media error hierarchy
- `app/modules/media/repository.py`: Media asset repository - the only place that queries media_assets
- `app/modules/media/router.py`: Media HTTP router (organization- and channel-scoped)
- `app/modules/media/schemas.py`: Media API schemas
- `app/modules/media/service.py`: Media service - upload, retrieval, and attachment business logic
- `app/modules/media/validation.py`: Media type validation (pure)

**app/modules/media/storage**
- `app/modules/media/storage/__init__.py`: Pluggable media storage backends
- `app/modules/media/storage/base.py`: Storage provider interface
- `app/modules/media/storage/local.py`: Local filesystem storage provider (development only)
- `app/modules/media/storage/s3.py`: Dependency-light S3-compatible media storage with AWS Signature V4

**app/modules/monetization**
- `app/modules/monetization/__init__.py`: Monetization module (Stage 25)
- `app/modules/monetization/agent.py`: RevenueAgent - analytics-grounded pricing recommendations
- `app/modules/monetization/enums.py`: Monetization enums (dependency-free)
- `app/modules/monetization/errors.py`: Typed errors for the monetization module
- `app/modules/monetization/parsing.py`: Parsing &amp; validation of the AI pricing response (pure)
- `app/modules/monetization/repository.py`: Monetization persistence (thin data access, org-scoped)
- `app/modules/monetization/router.py`: Monetization HTTP router (organization-scoped)
- `app/modules/monetization/schemas.py`: Monetization schemas: CRUD payloads + validated AI pricing output
- `app/modules/monetization/service.py`: Monetization service - CRUD, centralized financial-action gating, audits

**app/modules/notifications**
- `app/modules/notifications/__init__.py`: Durable notification outbox module
- `app/modules/notifications/repository.py`: Persistence for the durable notification outbox
- `app/modules/notifications/service.py`: Durable notification enqueueing and delivery with encrypted payloads

**app/modules/nova_core**
- `app/modules/nova_core/__init__.py`: Nova Core - the central decision and safety layer
- `app/modules/nova_core/actions.py`: Action definitions, their required permissions, and default risk
- `app/modules/nova_core/decision.py`: Decision result schema and the plain audit record
- `app/modules/nova_core/repository.py`: Database-backed audit sink for Nova Core
- `app/modules/nova_core/risk.py`: Risk levels and ordering helpers
- `app/modules/nova_core/rules.py`: The Nova Core rule engine
- `app/modules/nova_core/service.py`: Nova Core service - the central safe autonomy gate

**app/modules/organizations**
- `app/modules/organizations/__init__.py`: Organizations &amp; RBAC module (service / repository / router)
- `app/modules/organizations/repository.py`: Organization repository - the only place that talks to the database for
- `app/modules/organizations/router.py`: Organizations &amp; members HTTP router
- `app/modules/organizations/schemas.py`: Pydantic schemas for the organizations / RBAC module
- `app/modules/organizations/service.py`: Organization / RBAC service - business logic and invariants

**app/modules/publishing**
- `app/modules/publishing/__init__.py`: Publishing module - safe scheduling and Telegram publishing
- `app/modules/publishing/enums.py`: Publishing enums - dependency-free for standalone testing
- `app/modules/publishing/errors.py`: Publishing module error hierarchy
- `app/modules/publishing/repository.py`: Publishing repository - data access layer
- `app/modules/publishing/router.py`: Publishing API router (organization-scoped, thin)
- `app/modules/publishing/schemas.py`: Pydantic schemas for the publishing module
- `app/modules/publishing/service.py`: Publishing service - all safety gates live here
- `app/modules/publishing/telegram_publisher.py`: Isolated Telegram publishing service
- `app/modules/publishing/worker.py`: Publishing worker - polls for due jobs and executes them

**app/modules/research**
- `app/modules/research/__init__.py`: Research module - Nova's first collection agent
- `app/modules/research/agent.py`: ResearchAgent - reads active sources and creates deduplicated candidates
- `app/modules/research/entries.py`: CollectedEntry - a normalized candidate produced by a collector
- `app/modules/research/filters.py`: Deterministic and AI-assisted filtering for collected entries
- `app/modules/research/router.py`: Research collection trigger (organization- and channel-scoped)
- `app/modules/research/schemas.py`: Response schemas for a collection run

**app/modules/research/collectors**
- `app/modules/research/collectors/__init__.py`: Collectors turn a content source into normalized CollectedEntry objects
- `app/modules/research/collectors/base.py`: Collector interface shared by every source type
- `app/modules/research/collectors/base_collector.py`: BaseCollector: template-method pipeline shared by all source collectors
- `app/modules/research/collectors/custom.py`: Custom collector registry: plug in project-specific collectors
- `app/modules/research/collectors/google_news.py`: Google News collector: keyword query -&gt; Google News RSS -&gt; entries
- `app/modules/research/collectors/http_safety.py`: Shared response-size guards for untrusted HTTP collector payloads
- `app/modules/research/collectors/manual.py`: Manual collector: operator-provided content (no network, no retries)
- `app/modules/research/collectors/rest_api.py`: REST API collector: JSON endpoints -&gt; entries via a field mapping
- `app/modules/research/collectors/rss.py`: RSS/Atom collector
- `app/modules/research/collectors/telegram_channel.py`: Telegram channel collector using the public ''t.me/s/&lt;channel&gt;'' preview
- `app/modules/research/collectors/url_safety.py`: SSRF-safe validation for collector network destinations
- `app/modules/research/collectors/website.py`: Website collector: extract readable text blocks from any HTML page

**app/modules/storefront**
- `app/modules/storefront/__init__.py`: Telegram-authenticated NOVA storefront and shared plan administration
- `app/modules/storefront/router.py`: Public NOVA catalogue, Telegram Mini App account, checkout and admin API
- `app/modules/storefront/static.py`: Static Mini App files with the framing policy required by Telegram Web
- `app/modules/storefront/usage.py`: Server-priced custom plans, immutable quote snapshots, and official XTR checkout
- `app/modules/storefront/web_auth.py`: Explicit Telegram approval for browser sessions, independent of BotFather domain setup
- `app/modules/storefront/web_trial.py`: Authenticated webapp opening is the sole automatic trial activation path
- `app/modules/storefront/workspace.py`: Owned customer resources, role-scoped administration, and durable bot actions

**app/modules/storefront/web/assets**
- `app/modules/storefront/web/assets/storefront.js`: JavaScript helpers: $, el, stripUiPeriods, errorText
- `app/modules/storefront/web/assets/workspace.js`: JavaScript helpers: ws, send, $, e

**app/modules/strategy**
- `app/modules/strategy/__init__.py`: Content Strategy Engine module
- `app/modules/strategy/agent.py`: Strategy Agent - builds context and produces a validated strategy draft
- `app/modules/strategy/automation.py`: Defines run_weekly_posting_optimizer_once
- `app/modules/strategy/enums.py`: Strategy module enums - dependency-free
- `app/modules/strategy/errors.py`: Strategy module error hierarchy
- `app/modules/strategy/generator.py`: Strategy generators
- `app/modules/strategy/guardrails.py`: Content guardrails (pure) - forbidden-topic matching for strategy output
- `app/modules/strategy/optimizer.py`: Defines OptimizedSlot, HourBucketScore, WindowSet, PostingInsights
- `app/modules/strategy/parsing.py`: Parsing &amp; validation of the AI strategy response (pure)
- `app/modules/strategy/periods.py`: Pure helpers for computing a strategy plan's coverage period
- `app/modules/strategy/repository.py`: Strategy repository - the only place that queries strategy_plans /
- `app/modules/strategy/router.py`: Strategy HTTP router (organization- and channel-scoped)
- `app/modules/strategy/schemas.py`: Pydantic schemas for the strategy module
- `app/modules/strategy/service.py`: Defines StrategyService

**app/modules/telegram**
- `app/modules/telegram/__init__.py`: Isolated Telegram integration module (aiogram 3.x)
- `app/modules/telegram/admin_identity.py`: Small, dependency-light source of the Telegram owner/admin identities
- `app/modules/telegram/admin_panel.py`: Complete professional admin panel for Nova Telegram bot
- `app/modules/telegram/ahura_panel.py`: Ahura God-Mode Secret Admin Panel for Nova Telegram Bot
- `app/modules/telegram/ai_client.py`: Provider-agnostic AI client for Nova
- `app/modules/telegram/ai_schedule.py`: AI active-hours (wake/sleep) scheduling for the Nova Telegram bot
- `app/modules/telegram/approval_flow.py`: Telegram approval flow (UI layer over the Approval System)
- `app/modules/telegram/backend_api.py`: Reliable internal client for the NOVA Backend API
- `app/modules/telegram/backup.py`: Encrypted database backups for the Telegram product
- `app/modules/telegram/bot.py`: Telegram bot bootstrap (development mode)
- `app/modules/telegram/bot_settings.py`: Dynamic bot settings stored in the database and editable from the admin panel
- `app/modules/telegram/callbacks.py`: Structured, validated Telegram callback data
- `app/modules/telegram/channel_memory.py`: Channel Episodic &amp; Semantic Memory with Massive Fact Distillation for Nova
- `app/modules/telegram/client.py`: Telegram client abstraction
- `app/modules/telegram/comment_learner.py`: Comment Machine Learning and Semantic Memory for Nova
- `app/modules/telegram/content_gate.py`: Single decision point for every item before it reaches a destination
- `app/modules/telegram/default_texts.py`: All user-facing bot texts, editable one-by-one from the admin panel
- `app/modules/telegram/destination_hosts.py`: Dedicated Destination Host Management for Nova Telegram Bot
- `app/modules/telegram/fact_checker.py`: Comprehensive Multi-Source News Fact-Checking &amp; Destination Thematic Relevance for Nova
- `app/modules/telegram/feeds_ui.py`: User-facing UI for automatic web/API content feeds (🌐 منابع خودکار)
- `app/modules/telegram/handlers.py`: Thin Telegram command handlers
- `app/modules/telegram/headless_browser.py`: Bounded hjs-backed renderer for dynamic public pages
- `app/modules/telegram/humanizer.py`: Global Humanizer &amp; Anti-AI Content Transformer for Nova
- `app/modules/telegram/jev_layer.py`: TypeSafe JEV System One intelligence layer for Nova
- `app/modules/telegram/keyboards.py`: Inline keyboard builders (aiogram-free)
- `app/modules/telegram/ledger.py`: PRD ledger - per-purchase subscription records, audit log, retention &amp; display TZ
- `app/modules/telegram/logging.py`: Structured logging for Telegram events
- `app/modules/telegram/onboarding.py`: Channel onboarding flow (messages + navigation only)
- `app/modules/telegram/registration.py`: Registration flow - professional onboarding wizard
- `app/modules/telegram/service.py`: Telegram service layer
- `app/modules/telegram/sources_catalog.py`: Default content-source catalog + fetchers
- `app/modules/telegram/state_store.py`: Crash-safe persistent mapping for Telegram conversation state
- `app/modules/telegram/subscription_pricing.py`: Deterministic usage-based subscription quotation
- `app/modules/telegram/telethon_gemini.py`: Production Telegram runtime for Nova
- `app/modules/telegram/texts.py`: Centralized, localization-ready Telegram UI texts
- `app/modules/telegram/usage_accounting.py`: Provider-reported token accounting and deterministic hard-limit decisions
- `app/modules/telegram/usage_runtime.py`: Defines QuotaExceeded, reserve, reconcile
- `app/modules/telegram/user_memory.py`: User-Aware Comment Memory and Conversational Identity System for Nova
- `app/modules/telegram/utils.py`: Pure, dependency-free helpers shared by the Telegram runtime modules
- `app/modules/telegram/webapp_bridge.py`: Authenticated browser approval and durable, allowlisted web actions in the bot runtime

**app/modules/users**
- `app/modules/users/__init__.py`: Users &amp; authentication module (service / repository / router)
- `app/modules/users/repository.py`: User repository - the only place that talks to the database for users
- `app/modules/users/router.py`: Auth &amp; users HTTP routers
- `app/modules/users/schemas.py`: Pydantic schemas for the users/auth module
- `app/modules/users/service.py`: User/auth service - business logic for identity

**app/modules/verification**
- `app/modules/verification/__init__.py`: Verification module - Nova's content Verification Agent
- `app/modules/verification/agent.py`: Verification Agent - conservative pre-AI content verification
- `app/modules/verification/analyzer.py`: Content analyzers
- `app/modules/verification/decision.py`: Conservative decision logic (pure)
- `app/modules/verification/errors.py`: Verification error hierarchy
- `app/modules/verification/parsing.py`: Parsing &amp; validation of the AI analysis response (pure)
- `app/modules/verification/router.py`: Verification HTTP router (organization- and channel-scoped)
- `app/modules/verification/schemas.py`: Structured schemas for verification

**app/modules/writer**
- `app/modules/writer/__init__.py`: Writer module - Nova's Writer Agent
- `app/modules/writer/agent.py`: Writer Agent - professional, channel-ready draft generation
- `app/modules/writer/attribution.py`: Source attribution (pure)
- `app/modules/writer/errors.py`: Writer error hierarchy
- `app/modules/writer/generator.py`: Draft generators
- `app/modules/writer/guardrails.py`: Content guardrails (pure)
- `app/modules/writer/parsing.py`: Parsing &amp; validation of the AI draft response (pure)
- `app/modules/writer/router.py`: Writer HTTP router (organization- and channel-scoped)
- `app/modules/writer/schemas.py`: Schemas and options for the Writer Agent
- `app/modules/writer/templating.py`: Output templating (pure)

**app/scheduler**
- `app/scheduler/__init__.py`: NOVA scheduler package
- `app/scheduler/core.py`: Asyncio job scheduler with retry, backoff, timezone support, and metrics
- `app/scheduler/jobs.py`: Production scheduler jobs

**app/worker**
- `app/worker/__init__.py`: Worker queue foundation
- `app/worker/queue.py`: A minimal, dependency-light async worker queue

**migrations**
- `migrations/env.py`: Alembic environment (async)
- `migrations/version_compat.py`: Compatibility repair for Alembic revision IDs stored by older releases

**migrations/versions**
- `migrations/versions/0001_baseline.py`: Alembic database revision: baseline (empty)
- `migrations/versions/0002_users.py`: Alembic database revision: users table
- `migrations/versions/0003_organizations.py`: Alembic database revision: organizations and organization_members tables
- `migrations/versions/0004_channels.py`: Alembic database revision: channels table
- `migrations/versions/0005_digital_twins.py`: Alembic database revision: digital_twins table
- `migrations/versions/0006_ai_usage.py`: Alembic database revision: ai_usage_logs table
- `migrations/versions/0007_audit_logs.py`: Alembic database revision: decision_logs table (Nova Core safety decisions)
- `migrations/versions/0008_audit_logs_general.py`: Alembic database revision: audit_logs table (general production audit trail)
- `migrations/versions/0009_content_sources.py`: Alembic database revision: content_sources table
- `migrations/versions/0010_content_items.py`: Alembic database revision: content_items table
- `migrations/versions/0011_content_item_verification.py`: Alembic database revision: add verification column to content_items
- `migrations/versions/0012_media_assets.py`: Alembic database revision: media_assets table
- `migrations/versions/0013_approval_requests.py`: Alembic database revision: approval_requests table
- `migrations/versions/0014_publishing_jobs.py`: Alembic database revision: 0014 - publishing_jobs table
- `migrations/versions/0015_analytics.py`: Alembic database revision: 0015 - analytics_metrics, content_performance_snapshots, channel_growth_snapshots
- `migrations/versions/0016_strategy.py`: Alembic database revision: 0016 - strategy_plans, calendar_slots
- `migrations/versions/0017_growth_reports.py`: Alembic database revision: 0017 - growth_reports
- `migrations/versions/0018_community.py`: Alembic database revision: 0018 - community_messages
- `migrations/versions/0019_monetization.py`: Alembic database revision: 0019 - advertisers, campaigns, revenue_records
- `migrations/versions/0020_billing.py`: Alembic database revision: 0020 - plans, subscriptions, usage_counters
- `migrations/versions/0021_performance_indexes.py`: Alembic database revision: 0021 - performance indexes
- `migrations/versions/0022_billing_invoices_payments.py`: Alembic database revision: 0022 - billing invoices and payments
- `migrations/versions/0023_telegram_product.py`: Alembic database revision: 0023 - Telegram product tables
- `migrations/versions/0024_smart_content_filters.py`: Alembic database revision: 0024 - Complete Telegram runtime schema and smart content filters
- `migrations/versions/0025_content_delivery_limits.py`: Alembic database revision: 0025 - Per-feed character and delivery interval limits
- `migrations/versions/0026_telegram_stars.py`: Alembic database revision: 0026 - Telegram Stars payment ledger
- `migrations/versions/0027_star_referral_discounts.py`: Alembic database revision: 0027 - Referral point discounts for Telegram Stars
- `migrations/versions/0028_admin_star_rate.py`: Alembic database revision: 0028 - Snapshot the Star rate manually registered by a primary admin
- `migrations/versions/0029_prd_user_payment_ledger.py`: Alembic database revision: PRD: user profile columns, subscription ledger, audit logs, backup registry
- `migrations/versions/0030_financial_integrity.py`: Alembic database revision: Financial integrity constraints and idempotency
- `migrations/versions/0031_billing_payment_idempotency.py`: Alembic database revision: Billing payment external-reference idempotency
- `migrations/versions/0032_user_token_version.py`: Alembic database revision: Add account token version for immediate token revocation
- `migrations/versions/0033_content_search_fts.py`: Alembic database revision: Add PostgreSQL full-text index for channel content search
- `migrations/versions/0034_notification_outbox.py`: Alembic database revision: Add durable encrypted notification outbox
- `migrations/versions/0035_notification_stale_index.py`: Alembic database revision: Index stale notification claims
- `migrations/versions/0036_bot_user_attribution.py`: Alembic database revision: Add per-subscriber host and bot attribution handles
- `migrations/versions/0037_audit_hash_chain.py`: Alembic database revision: Add tamper-evident HMAC chain fields to general audit logs
- `migrations/versions/0038_audit_integrity_key_id.py`: Alembic database revision: Add key identity for rotating audit integrity keys
- `migrations/versions/0039_support_tickets.py`: Alembic database revision: Add persistent Telegram support tickets
- `migrations/versions/0040_referral_purchase_reward.py`: Alembic database revision: Grant referral points only after the referred user's first paid purchase
- `migrations/versions/0041_ref_contract.py`: Alembic database revision: Versioned clickwrap evidence and contract-bound referral rewards
- `migrations/versions/0042_ref_identity.py`: Alembic database revision: Snapshot both Telegram parties on referral contract acceptance
- `migrations/versions/0043_ref_security.py`: Alembic database revision: Cryptographically signed, append-only referral contract evidence
- `migrations/versions/0044_referral_cashout.py`: Alembic database revision: Cash withdrawal for purchase-triggered referral points
- `migrations/versions/0045_usage_based_subscriptions.py`: Alembic database revision: Usage-based subscription pricing snapshots and entitlements
- `migrations/versions/0046_referral_percentage_wallet.py`: Alembic database revision: Percentage referral reward snapshots for the cash wallet
- `migrations/versions/0047_ai_usage_fraud_controls.py`: Alembic database revision: AI usage ledger and referral fraud controls
- `migrations/versions/0048_production_usage_controls.py`: Alembic database revision: Production usage, subscription and review controls
- `migrations/versions/0049_custom_footer.py`: Alembic database revision: Add custom_footer to bot_users, content_feeds, channel_mirrors
- `migrations/versions/0050_webapp_storefront.py`: Alembic database revision: Add idempotent Telegram Mini App Stars checkout fields
- `migrations/versions/0051_web_workspace.py`: Alembic database revision: Persistent web operations and support threads
- `migrations/versions/0052_webapp_trial.py`: Alembic database revision: One-time trials activated exclusively by authenticated webapp opening
- `migrations/versions/0053_channel_whitelist_review.py`: Alembic database revision: Verified destination ID allowlist and channel-admin publication reviews

**scripts**
- `scripts/backup-vps.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `scripts/check-migration-chain.py`: Dependency-free Alembic chain and revision-length audit
- `scripts/deploy-vps.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `scripts/preflight-vps.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename
- `scripts/verify-postgres-backend.sh`: Shell operator or deployment helper; exact purpose is indicated by the filename

**tests**
- `tests/__init__.py`: Python test/support module
- `tests/conftest.py`: Python test/support module
- `tests/test_admin.py`: Test module covering test_mask_email, test_normal_user_denied, test_org_owner_denied
- `tests/test_admin_panel_button_integrity.py`: Test module covering test_all_admin_callback_patterns_are_unique, test_main_and_parallel_button_actions_have_handlers, test_health_uses_real_ai_client
- `tests/test_admin_ui_edit_mode.py`: Test module covering test_edit_mode_flag_exists_and_defaults_off, test_pens_are_rendered_only_in_edit_mode, test_single_toggle_button_is_offered_to_ad
- `tests/test_ai.py`: Test module covering test_memory_sink_satisfies_protocol, test_mock_provider_is_deterministic, test_provider_selection_and_defaults
- `tests/test_ai_connection_endpoint.py`: Test module covering test_both_api_values_are_required, test_endpoint_is_validated_and_encrypted, test_endpoint_is_provider_agnostic
- `tests/test_analytics.py`: Test module covering test_record_content_metric_via_api, test_record_channel_growth_snapshot, test_record_business_metric_requires_org_update_permissi
- `tests/test_approvals.py`: Test module covering test_policy_permission_mapping, test_policy_can_review_by_role, test_policy_default_risk
- `tests/test_arvancloud_auth.py`: Test module covering test_arvan_gateway_uses_apikey_scheme, test_prefixed_key_is_not_duplicated, test_standard_provider_keeps_bearer
- `tests/test_attribution_host.py`: Test module covering test_shared_resolver_exists, test_dash_resolves_to_own_username_at_account_level, test_handle_existence_is_verified_against_teleg
- `tests/test_auth.py`: Test module covering test_password_is_hashed, test_long_passwords_do_not_collapse_after_bcrypt_72_byte_boundary, test_register_user
- `tests/test_backup_manifest_integrity.py`: Test module covering test_backup_manifest_contains_dump_size_and_sha256, test_backup_archive_rejects_unknown_members
- `tests/test_backup_safety.py`: Test module covering test_local_restore_does_not_require_distributed_control, test_production_restore_is_disabled_by_default, test_production_restore_
- `tests/test_billing.py`: Test module covering test_list_plans_seeds_defaults, test_assign_plan_and_subscription_detail, test_assign_unknown_plan_404
- `tests/test_billing_payments.py`: Test module covering test_invoice_creation_and_payment_flow
- `tests/test_bot_settings_secrets.py`: Test module covering test_secret_key_detection, test_secret_encrypted_at_rest, test_non_secret_stored_plaintext
- `tests/test_cache_layer.py`: Test module covering test_set_get_and_metrics, test_ttl_expiry_returns_none_and_counts_miss, test_delete_and_prefix_invalidation_is_tenant_scoped
- `tests/test_channel_admin_review.py`: Test module covering test_bot_admin_cannot_approve_without_admin_rights_in_target_channel, test_feed_delivery_fails_closed_without_verified_destinatio
- `tests/test_channel_memory_duplicate_review.py`: Test module covering test_empty_channel_memory_is_new_and_does_not_call_ai, test_confident_memory_match_is_suppressed, test_ambiguous_memory_match_req
- `tests/test_channel_review_migration.py`: Test module covering test_upgrade_creates_review_tables_and_backfills_verified_destinations
- `tests/test_channels.py`: Test module covering test_create_and_get_channel, test_list_channels, test_update_channel_settings
- `tests/test_community_agent.py`: Test module covering test_parse_valid_analysis, test_parse_invalid_analysis, test_question_and_sensitivity_heuristics
- `tests/test_completion_hardening.py`: Test module covering test_field_encryption_detects_tampering, test_production_insecure_defaults_fail_closed, test_compose_has_writable_persistent_back
- `tests/test_content_gate.py`: Test module covering test_obvious_persian_ad_is_detected_for_free, test_plain_news_is_not_flagged_as_ad, test_news_about_a_company_is_not_an_ad
- `tests/test_content_pipeline.py`: Test module covering test_deterministic_hash, test_hash_detects_difference, test_state_machine_valid_and_invalid
- `tests/test_content_sources.py`: Test module covering test_is_valid_http_url, test_validate_type_url, test_find_secret_like_keys
- `tests/test_db.py`: Test module covering test_health_db_endpoint, test_session_creation, test_sessionmaker_is_reusable
- `tests/test_db_bootstrap.py`: Test module covering test_is_sqlite_url, test_resolve_database_url_prefers_environment, test_current_environment_is_normalized
- `tests/test_destination_ownership.py`: Test module covering test_ownership_helper_exists, test_resolver_accepts_and_enforces_requester, test_every_call_site_passes_the_requester
- `tests/test_digital_twins.py`: Test module covering test_create_digital_twin, test_update_onboarding_profile, test_reject_invalid_data
- `tests/test_full_lifecycle_connections.py`: Test module covering test_every_daemon_bootstraps_the_database_before_work, test_scheduler_is_single_optimizer_owner, test_health_paths_use_rollback_o
- `tests/test_google_news_freshness.py`: Test module covering test_research_google_news_url_and_parser_are_limited_to_one_day, test_fact_checker_google_rss_excludes_old_and_undated_items, tes
- `tests/test_growth_agent.py`: Test module covering test_parse_valid_ai_growth_report, test_parse_invalid_ai_growth_report, test_extraction_aggregates_data
- `tests/test_hardening_regressions.py`: Test module covering test_validate_production_env_rejects_missing_secret, test_validate_production_env_warns_on_insecure_default, test_client_identifi
- `tests/test_health.py`: Test module covering test_health_root, test_health_versioned
- `tests/test_lifecycle_regressions.py`: Test module covering test_environment_selection_is_explicit, test_scheduler_signal_drains_all_tasks, test_scheduler_surfaces_crashed_loop
- `tests/test_link_fallback_breaking_policy.py`: Test module covering test_mixed_link_text_falls_back_without_false_failure, test_fetch_has_two_attempts_and_extraction_fallbacks, test_breaking_label_
- `tests/test_media.py`: Test module covering test_sniff_known_types, test_resolve_media_type_ok_and_reject, test_sanitize_filename_is_path_free
- `tests/test_metrics.py`: Test module covering test_counter_increments_with_labels, test_gauge_set_overwrites, test_render_prometheus_text_format
- `tests/test_migrations.py`: Test module covering test_migrations_upgrade_and_downgrade
- `tests/test_monetization.py`: Test module covering test_parse_valid_pricing, test_parse_invalid_pricing, test_build_pricing_context_grounded
- `tests/test_nova_core.py`: Test module covering test_memory_sink_satisfies_protocol, test_low_risk_action_is_allowed, test_critical_action_requires_approval
- `tests/test_observability.py`: Test module covering test_mask_sensitive_fields, test_error_envelope_builder, test_json_formatter_includes_request_id
- `tests/test_offline_hardening.py`: Test module covering test_all_python_sources_parse, test_migration_graph_has_single_expected_head, test_production_images_are_non_root_and_have_health
- `tests/test_organizations.py`: Test module covering test_create_organization, test_creator_becomes_owner, test_add_member
- `tests/test_owner_title_userbot_defaults.py`: Test module covering test_primary_admin_is_immutable_owner, test_ui_and_text_edits_require_primary, test_content_prompt_forbids_invented_titles
- `tests/test_pagination_regressions.py`: Test module covering test_channel_list_pagination, test_content_sources_list_pagination
- `tests/test_paid_subscriber_panel_integrity.py`: Test module covering test_callback_patterns_are_unique, test_parallel_subscriber_buttons_have_handlers, test_paid_actions_reauthorize_callbacks_and_te
- `tests/test_performance.py`: Test module covering test_page_params_clamping, test_page_from_query, test_page_no_more
- `tests/test_postgres_migration_chain_hardening.py`: Test module covering test_chain_audit_passes, test_last_three_standard_names, test_all_revision_ids_fit_alembic_version
- `tests/test_publishing.py`: Test module covering test_create_publishing_job_automatic, test_duplicate_idempotency_key_returns_409, test_assistant_mode_blocks_creation
- `tests/test_redis_facade.py`: Test module covering test_status_when_unconfigured, test_rate_limit_fixed_window_fallback, test_distributed_lock_mutual_exclusion
- `tests/test_referral_cash_withdrawal_policy.py`: Test module covering test_reward_still_requires_first_paid_purchase, test_points_never_discount_new_purchases, test_admin_configures_value_and_minimum
- `tests/test_referral_contract.py`: Test module covering test_clickwrap_evidence_is_immutable_and_versioned, test_link_and_rewards_require_current_contract, test_admin_can_edit_contract_
- `tests/test_referral_percentage_wallet.py`: Test module covering test_referral_reward_uses_purchase_percentage_and_point_value, test_wallet_is_cash_only_and_supports_withdrawal, test_host_is_man
- `tests/test_referral_purchase_rewards.py`: Test module covering test_join_records_relationship_without_granting_points, test_first_paid_purchase_is_the_only_reward_trigger, test_reward_marker_i
- `tests/test_registration_flow.py`: Test module covering test_registration_dispatcher_accepts_every_real_button, test_clean_card_strips_separators, test_callback_ignored_without_registra
- `tests/test_release_hardening_v0331.py`: Test module covering test_httpx_is_a_runtime_dependency, test_paid_features_fail_closed_without_subscription, test_response_guard_rejects_stream_over_
- `tests/test_release_hardening_v03310.py`: Test module covering test_active_key_postcondition_changes_only_after_rewrap, test_rotation_service_enforces_postconditions_before_flush, test_postgre
- `tests/test_release_hardening_v03311.py`: Test module covering test_bounded_text_is_deterministic_and_distinguishes_long_inputs, test_request_id_acceptance_matches_database_column, test_audit_
- `tests/test_release_hardening_v03312.py`: Test module covering test_hmac_chain_detects_payload_tampering_and_forks, test_production_requires_dedicated_audit_integrity_key, test_repository_seri
- `tests/test_release_hardening_v03313.py`: Test module covering test_mixed_key_chain_verifies_during_rotation, test_key_id_is_authenticated, test_schema_repository_and_operator_keep_key_identit
- `tests/test_release_hardening_v03314.py`: Test module covering test_signed_checkpoint_detects_full_scope_deletion, test_checkpoint_signature_and_atomic_persistence, test_operator_tools_require
- `tests/test_release_hardening_v03315.py`: Test module covering test_object_lock_headers_are_covered_by_sigv4, test_production_store_rejects_plain_http, test_publish_requires_compliance_retenti
- `tests/test_release_hardening_v03316.py`: Test module covering test_checkpoint_history_is_monotonic_and_replay_resistant, test_predecessor_digest_is_authenticated, test_s3_backend_enumerates_v
- `tests/test_release_hardening_v0332.py`: Test module covering test_secure_production_environment_is_accepted, test_sqlite_is_rejected_in_production_validation, test_weak_jwt_secret_is_rejecte
- `tests/test_release_hardening_v0333.py`: Test module covering test_heartbeat_reports_missing_and_fresh_files, test_heartbeat_reports_stale_file, test_compose_shares_backup_volume_between_bot_
- `tests/test_release_hardening_v0334.py`: Test module covering test_dead_letter_can_be_replayed_and_processed, test_malformed_dead_letter_is_preserved, test_readiness_returns_503_on_dependency
- `tests/test_release_hardening_v0335.py`: Test module covering test_valid_sqlite_native_backup_passes_integrity_check, test_corrupt_sqlite_native_backup_is_rejected, test_backup_creation_verif
- `tests/test_release_hardening_v0336.py`: Test module covering test_declared_oversized_body_is_rejected_before_app, test_chunked_body_cannot_bypass_limit, test_bounded_body_is_replayed_unchang
- `tests/test_release_hardening_v0337.py`: Test module covering test_v3_ciphertext_survives_zero_downtime_key_rotation, test_removed_rotation_key_fails_closed, test_v2_authenticated_values_use_
- `tests/test_release_hardening_v0338.py`: Test module covering test_rewrap_allows_previous_key_to_be_retired, test_malformed_v3_envelope_requires_reencryption_and_fails_closed, test_settings_r
- `tests/test_release_hardening_v0339.py`: Test module covering test_prefixed_outbox_payload_rewraps_and_survives_retirement, test_plaintext_outbox_payload_fails_closed, test_rotation_service_c
- `tests/test_research_agent.py`: Test module covering test_parse_mocked_feed, test_parse_invalid_feed_raises, test_keyword_filters
- `tests/test_scheduler_core.py`: Test module covering test_job_definition_requires_exactly_one_trigger, test_manual_trigger_success_and_metrics, test_retry_with_backoff_then_success
- `tests/test_security.py`: Test module covering test_rate_limiter_allows_normal_requests, test_rate_limiter_blocks_excess, test_rate_limiter_reset
- `tests/test_smart_filters.py`: Test module covering test_persian_arabic_normalization_and_typo_examples, test_latin_typo_examples, test_exclude_always_wins
- `tests/test_source_catalog_hardening.py`: Test module covering test_catalog_keys_and_fetchers_are_consistent, test_all_curated_homepages_are_attached, test_manual_source_url_is_canonical_and_s
- `tests/test_source_delivery_hardening.py`: Test module covering test_custom_attribution_handle_validation, test_attribution_settings_are_per_subscriber_and_migrated, test_signature_cannot_be_tr
- `tests/test_source_end_to_end_hardening.py`: Test module covering test_api_registration_rejects_unexecutable_source_configs, test_api_urls_are_canonical_and_ssrf_safe, test_destination_is_verifie
- `tests/test_star_payment_model.py`: Test module covering test_star_payment_has_pricing_snapshot_columns, test_star_payment_instantiation_keeps_values, test_bot_setting_key_value_roundtri
- `tests/test_strategy.py`: Test module covering test_parse_valid_ai_strategy, test_parse_invalid_ai_strategy, test_compute_period_daily_weekly_monthly
- `tests/test_telegram.py`: Test module covering test_service_messages, test_start_handler_replies_with_service_message, test_help_handler_replies_with_service_message
- `tests/test_telegram_state_store.py`: Test module covering test_state_survives_restart_and_tracks_nested_changes, test_pop_is_persisted
- `tests/test_telegram_utils.py`: Test module covering test_aware_none_passthrough, test_aware_adds_utc_to_naive_datetimes, test_aware_keeps_existing_timezone
- `tests/test_telegram_ux.py`: Test module covering test_render_with_variables, test_render_missing_variable_raises, test_render_unknown_key_raises
- `tests/test_usage_subscription_pricing.py`: Test module covering test_more_usage_always_costs_more, test_price_scales_with_destinations_not_sources, test_quote_contains_immutable_inputs_and_toke
- `tests/test_user_panels_end_to_end_integrity.py`: Test module covering test_main_parallel_buttons_route, test_registration_buttons_and_numeric_steps_route, test_support_command_matches_callback_menu_a
- `tests/test_userbot_reconnect_guard.py`: Test module covering test_helper_reconnects_user_client, test_resolve_ref_uses_guard_for_user_client, test_mirror_registration_hides_raw_disconnected_
- `tests/test_v038_completeness.py`: Test module covering test_actual_usage, test_delivery_recovery, test_sources_filtered
- `tests/test_verification_agent.py`: Test module covering test_validate_structured_ai_response_ok, test_validate_structured_ai_response_clamps, test_validate_structured_ai_response_invali
- `tests/test_vps_release_integrity.py`: Test module covering test_compose_yaml_and_services, test_required_contract_secret_reaches_bot, test_production_security_secrets_are_required
- `tests/test_web_workspace.py`: Test module covering test_resource_columns_and_encryption_contract, test_customer_scope_idor_and_blocked_account, test_roles_and_write_whitelist_and_c
- `tests/test_webapp_trial.py`: Test module covering test_open_claims_once_and_never_extends_expired_trial, test_ineligible_and_existing_users_are_not_granted_or_overwritten, test_dy
- `tests/test_worker_queue.py`: Test module covering test_enqueue_dequeue_fifo, test_worker_processes_all_jobs, test_worker_error_does_not_stop_batch
- `tests/test_writer_agent.py`: Test module covering test_parse_valid_ai_draft, test_parse_invalid_ai_draft, test_normalize_hashtags_limit_and_clean

**tests/e2e**
- `tests/e2e/__init__.py`: Python test/support module
- `tests/e2e/test_core_flow.py`: Test module covering test_core_flow_register_to_publishing, test_multi_tenant_isolation, test_viewer_cannot_approve

**tests/integration**
- `tests/integration/__init__.py`: Python test/support module
- `tests/integration/test_service_readiness.py`: Test module covering test_postgresql_is_reachable, test_redis_is_reachable

</details>

## Important limits

- Channel whitelisting and a channel-admin review queue exist in the Python source and have focused tests, but live migration application and Telegram delivery were not independently verified here
- A repeated story can still pass when its content is materially changed or the duplicate check misses it, so no duplicate filter can promise perfect suppression
- Google News collector code requires a verifiable timestamp in the last 24 hours, but freshness from every other provider and real-time delivery have not been verified
- Trial activation is implemented on authenticated webapp open, and the source tests verify that bot registration or older bot calls cannot grant a trial, but production deployment was not independently exercised
- Crypto checkout is not connected to a payment provider and is not a completed purchase flow
- The production configuration lists Go scheduler and worker binaries whose source was unavailable for review, so their internal behavior is not documented here
- The dependency-free migration audit passed for all 53 revisions, while focused pytest tests could not be run because pytest is not installed in this documentation environment
- Some workspace routes and administrative screens still need end-to-end validation

This README describes the audited Python source and its existing tests, not a guarantee that every capability is enabled or currently healthy in production

## Public repository

This repository keeps only the README in its tracked source tree
The design preview pictures are attached to the [README previews release](https://github.com/ahurkkkkkkk/NOVA/releases/tag/readme-previews)
No bot source code, web app source code, credentials, customer data or runtime configuration is published here
