---
name: Run Rainforest tests and collect results
description: Kick off a Rainforest run for a set of tests or a run group, poll for completion, and pull the JUnit results — retrying failed tests when needed.
api: openapi/rainforest-qa-openapi-original.yml
operations: [post-runs, get-run, get-run-junit, post-run-rerun_failed, delete-run]
---

# Run Rainforest tests and collect results

Authenticate every request with the `CLIENT_TOKEN` header (your API token from the
Integrations settings page). Base URL: `https://app.rainforestqa.com/api`.

## Steps
1. **Start a run** — `post-runs` (`POST /1/runs`). Provide the tests to run
   (e.g. `tests`, `smart_folder_id`, or `run_group_id`) plus the target
   `environment_id`. Capture the returned run `id`.
2. **Poll the run** — `get-run` (`GET /1/runs/{run_id}`) until `state` reaches a
   terminal value (`complete`). Poll on a sensible interval; do not tight-loop.
3. **Fetch results** — `get-run-junit` (`GET /1/runs/{run_id}/junit`) for the
   JUnit XML summary of every test in the run.
4. **Retry failures (optional)** — `post-run-rerun_failed`
   (`POST /1/runs/{run_id}/rerun_failed`) to rerun only the failed tests.
5. **Cancel (optional)** — `delete-run` (`DELETE /1/runs/{run_id}`) to cancel a
   run in progress.

## Conventions
- Errors come back as `{"error": "..."}` with a 4xx status (400 bad params,
  401 unauthorized, 404 not found, 409 timing conflict — retry 409 after a short wait).
- List endpoints paginate with `page` and `page_size`.
- No idempotency key is supported; avoid blind retries of `post-runs`.
