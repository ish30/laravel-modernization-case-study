# Incident and Hotfix

## Incident summary

After the Laravel 13 production cutover, selected catalogue image endpoints redirected to an obsolete fallback administration host while configuration caching was active.

## Root cause

The affected controller read an environment variable directly at runtime instead of consuming a Laravel configuration value.

```php
// Fragile when configuration is cached
$baseUrl = env('ADMIN_ASSET_URL', 'https://obsolete.example');
```

The correction moved the value into configuration and consumed it through:

```php
$baseUrl = config('services.admin_asset.base_url');
```

## Corrective change

- Replaced direct environment reads
- Centralized the asset base URL
- Added regression tests for category, item, dimension, and colour-chart redirects
- Added a source guard preventing the controller from reading the environment directly
- Confirmed production homepage and catalogue health after correction
- Synchronized the live hotfix back into source control

## Lesson

A green CI run does not complete a framework upgrade. Production caching, runtime configuration, and post-cutover monitoring must also be exercised.
