# Upgrade Path

## Phase 0 — Reproducible foundations

- Pinned Node.js 22.16.0 and npm 10.9.2
- Committed and enforced the frontend lockfile
- Added `npm ci` and production asset build validation
- Added PHP 8.3 compatibility coverage

## Phase 1 — Laravel 10 to 11

- Established the Laravel 11 dependency baseline
- Created a sanitized representative legacy-schema baseline
- Aligned fixtures with legacy column types and required relationships
- Preserved catalogue sanitizer behavior
- Verified guest notification resolution
- Retained the secure WebXPay callback route
- Reached 203 tests and 1,077 assertions with zero failures

## Phase 2 — Laravel 11 to 12

- Laravel 11.55.0 to Laravel 12.64.0
- PHPUnit 10 to PHPUnit 11.5.56
- Carbon 2 to Carbon 3.13.1
- Migrated PHPUnit configuration
- Resolved Laravel 12 Blade parsing of JSON-LD `@context`

## Phase 3 — Laravel 12 to 13

- Laravel 13.20.0
- PHP requirement raised to `^8.3`
- Laravel Tinker 3.0.2
- PHPUnit 12.5.31
- Adopted `PreventRequestForgery`
- Migrated data providers to PHPUnit attributes
- Preserved callback exclusions and normal checkout protection
- Added a narrow cache deserialization allow-list

## Phase 4 — Integrated release candidate

Validated the combined application with PHP 8.3, MySQL 8, MariaDB 10.6, route/cache generation, and the Node 22 frontend build.

## Phase 5 — Staging

The release candidate ran in an isolated PHP 8.3 FPM staging application. The full suite passed with 209 tests and 1,101 assertions.

## Phase 6 — Production

The release moved through a controlled production cutover with backups, an isolated release directory, pre-switch verification, smoke tests, monitoring, and rollback criteria.
