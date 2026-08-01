---
name: Manage Rainforest environments before a run
description: List and create Rainforest environments so tests run against the right site/URL, then hand the environment_id to a run.
api: openapi/rainforest-qa-openapi-original.yml
operations: [get-environments, post-environments, get-tests, post-runs]
---

# Manage Rainforest environments before a run

Authenticate with the `CLIENT_TOKEN` header. Base URL: `https://app.rainforestqa.com/api`.

## Steps
1. **List environments** — `get-environments` (`GET /1/environments`) to find an
   existing environment and its `id`.
2. **Create one if needed** — `post-environments` (`POST /1/environments`) with a
   `name` (and site URLs); this also creates the corresponding site-environments.
   Capture the new `id`.
3. **Pick tests** — `get-tests` (`GET /1/tests`, paginated with `page`/`page_size`)
   to gather the test ids you want to run.
4. **Run against the environment** — `post-runs` (`POST /1/runs`) passing the
   `environment_id` from step 1/2 and the tests from step 3.

## Conventions
- Auth header: `CLIENT_TOKEN`. Errors: `{"error": "..."}` JSON envelope.
- Pagination: `page`, `page_size`.
