<div align="center">

# Laravel Modernization Case Study

### Production E-Commerce Platform · Laravel 10 → 13 · PHP 8.3

A security-conscious, test-driven modernization of a legacy Laravel e-commerce application while protecting checkout, payment, stock, catalogue, and customer workflows.

![Laravel](https://img.shields.io/badge/Laravel-10%20%E2%86%92%2013-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.1%20%E2%86%92%208.3-777BB4?style=for-the-badge&logo=php&logoColor=white)
![PHPUnit](https://img.shields.io/badge/PHPUnit-12.5.31-3C9CD7?style=for-the-badge)
![CI](https://img.shields.io/badge/CI-GitHub%20Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)

</div>

> **Confidentiality note:** This repository documents the engineering approach and verified outcomes. It intentionally excludes client source code, credentials, database records, production configuration, private URLs, and commercially sensitive information.

## Executive Summary

A mature Sri Lankan e-commerce platform was operating on Laravel 10 and PHP 8.1 with a legacy production schema, multiple payment integrations, stock-reservation logic, customer notification workflows, and long-lived application behavior that could not be casually rewritten.

The modernization was completed through staged checkpoints:

```text
Laravel 10 → Laravel 11 → Laravel 12 → Laravel 13 + PHP 8.3
        → Isolated staging → Controlled production cutover
        → Post-cutover monitoring and corrective hotfix
```

The release reached production. A configuration-cache-related catalogue asset redirect issue was detected after cutover and corrected with a narrow, regression-tested hotfix.

## Final Validated Stack

| Component | Final version / target |
|---|---:|
| Laravel Framework | 13.20.0 |
| PHP requirement | `^8.3` |
| Composer platform PHP | 8.3.0 |
| Laravel Tinker | 3.0.2 |
| PHPUnit | 12.5.31 |
| Carbon | 3.13.1 |
| Node.js | 22.16.0 |
| npm | 10.9.2 |
| Database validation | MySQL 8 and MariaDB 10.6 |

## Business-Critical Workflows Protected

- Guest and registered-customer checkout
- WebXPay callback and transaction verification
- PayHere mobile payment routes
- KOKO payment callbacks and notification effects
- Stock reservation, confirmation, and idempotency
- Order-customer snapshot handling
- Product and catalogue rendering
- Legacy database compatibility
- Route, configuration, and view cache generation
- Reproducible frontend asset builds

## Verified Quality Gates

- **Critical payment and stock suite:** 55 tests, 372 assertions
- **Representative full suite:** 209 tests, 1,101 assertions
- **PHP syntax validation:** 149 files
- **Route validation:** 70 registered routes
- **Composer packages installed in staging:** 113 locked packages
- MySQL 8 representative-schema validation passed
- MariaDB 10.6 full suite passed
- Node 22 frozen install and production build passed
- No tests were deliberately skipped, excluded, or weakened to obtain a green result

## Key Engineering Decisions

### Upgrade in controlled checkpoints

Each major Laravel version had its own compatibility and regression checkpoint. This reduced the debugging surface and made framework behavior changes attributable to a specific step.

### Test against a representative legacy schema

Historical migrations alone were not sufficient to reconstruct the live database accurately. A sanitized, structure-only schema baseline was created for isolated CI databases. It excluded customer, order, payment, catalogue, stock, session, credential, and secret data.

### Preserve payment and checkout boundaries

Payment callbacks were reviewed separately from ordinary checkout submission. Callback exclusions were preserved deliberately, while normal checkout remained protected by request-forgery middleware.

### Use locked dependencies in deployment

Production used the reviewed dependency graph:

```bash
composer install --no-dev --optimize-autoloader
```

`composer update` was treated as a development operation, not a production deployment command.

### Treat rollback as part of the release

The deployment plan required a fresh server snapshot, application and database backups, an isolated release directory, pre-switch verification, a controlled application/runtime switch, post-switch smoke testing, and explicit rollback conditions.

## Compatibility Issues Resolved

| Area | Issue | Resolution |
|---|---|---|
| Blade / SEO | Laravel 12 interpreted literal JSON-LD `@context` as a Blade directive | Escaped the source representation while preserving rendered JSON-LD |
| PHPUnit | PHPUnit 12 no longer accepted legacy data-provider conventions | Migrated providers to supported attributes |
| Request forgery | Laravel 13 middleware class changed | Adopted `PreventRequestForgery` and re-audited callback exclusions |
| Cache security | Framework changes required explicit cache deserialization policy | Added a narrow allow-list for required object classes |
| Legacy schema | Test fixtures assumed modern column types and optional relationships | Aligned fixtures with production-representative constraints |
| Routes | A shadowed legacy WebXPay declaration conflicted with the active secure callback | Retained one explicit secure POST callback contract |
| Frontend builds | Toolchain and lockfile were not reproducible | Pinned Node/npm and enforced `npm ci` plus production build |
| Config cache | Direct `env()` access outside configuration returned an obsolete fallback host | Moved asset host resolution to cached Laravel configuration |

## Post-Cutover Incident

After the Laravel 13 cutover, selected catalogue image endpoints redirected to an obsolete fallback host while Laravel configuration caching was enabled.

**Corrective action:** direct environment reads were replaced with `config(...)`, the asset base URL was centralized, regression tests were added for category, item, dimension, and colour-chart redirects, and production health was reconfirmed.

The lesson: **a framework upgrade is not finished when CI is green; production caching and runtime behavior must also be exercised and monitored.**

## Repository Guide

| Document | Purpose |
|---|---|
| [Executive Summary](docs/01-executive-summary.md) | Context, scope, constraints, and outcomes |
| [Upgrade Path](docs/02-upgrade-path.md) | Version-by-version modernization sequence |
| [Architecture & Risk Controls](docs/03-architecture-and-risk-controls.md) | Boundaries, safeguards, and release architecture |
| [Testing & CI](docs/04-testing-and-ci.md) | Test strategy, matrices, and quality gates |
| [Staging & Production Cutover](docs/05-staging-and-production-cutover.md) | Controlled validation and deployment approach |
| [Incident & Hotfix](docs/06-incident-and-hotfix.md) | Config-cache incident and corrective work |
| [Lessons Learned](docs/07-lessons-learned.md) | Reusable engineering lessons |
| [Evidence Register](docs/08-evidence-register.md) | Sanitized verification record |
| [Diagrams](docs/09-diagrams.md) | Mermaid architecture and release diagrams |

## Author

**Ishara Subasinghe**  
E-Commerce Growth Specialist · Laravel Developer · AI Automation

[GitHub Profile](https://github.com/ish30)
