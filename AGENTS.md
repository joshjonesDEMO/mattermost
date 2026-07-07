# AGENTS.md

Explicitly import subdirectory instruction files that must always be in context:
@server/AGENTS.md

## Pull Requests

When creating a pull request, follow `.github/PULL_REQUEST_TEMPLATE.md` exactly:

- Remove all `<!-- -->` comments.
- Omit sections that are not applicable (Ticket Link, Screenshots) — do not write N/A, just remove the header.
- The `#### Release Note` header and its "```release-note" fenced code block **must always be present** (WITHOUT escaping the ``` characters). Write `NONE` if the change has no API, schema, UI, or breaking changes.

## Cursor Cloud Agents

This repository has a checked-in Cloud Agent environment under `.cursor/`. Docker is started by `.cursor/scripts/cloud-agent-start.sh`; if Docker is unavailable in Cloud, treat that as an environment failure rather than falling back to snapshot assumptions.

The environment declares `mattermost/enterprise` as a Cursor multi-repo dependency. Cursor clones the repositories as siblings, so `server/Makefile` can use its default `../../enterprise` path; the install hook does not clone or symlink enterprise.

When the `enterprise` sibling checkout is absent (e.g. it is private and unavailable), the server still builds, lints, tests, and runs as a "team" build — `make setup-go-work` simply omits enterprise from `go.work` and `BUILD_ENTERPRISE_READY` stays `false`. In that case set `CLOUD_AGENT_SKIP_ENTERPRISE=true`, otherwise `cloud-agent-install.sh` (`set -e`) aborts at `verify_enterprise_checkout`. To run without Docker Hub credentials, prefer `ENABLED_DOCKER_SERVICES='postgres redis' make run-server` for the API on `:8065` and `cd webapp && make run` for the webpack dev build the server serves.
