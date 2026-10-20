# pokerService

A local Pokerlost FastAPI foundation. It provides health, canonical root/OpenAPI infrastructure, and fail-closed authorization support; no business operation is implemented.

## 🚀 Run

Python 3.14+ and access to the pinned private `pypokerlost` source are required to install. The installed wheel includes the SDK as a runtime dependency; it does not need an adjacent checkout. No database URL or outbound credential is needed for this default render.

```bash
uv lock
uv sync --locked
uv run --locked uvicorn pokerservice.app:create_app --factory --host 127.0.0.1 --port 8000
```

`GET /` uses `PokerlostResponse[None]`; `GET /getOpenApi` returns the unmodified OpenAPI document. `/health/live` and `/health/ready` are infrastructure probes. Read `SERVICE.md` for this service's data boundary, [architecture](docs/architecture.md) for the foundation lifecycle, and `AGENTS.md` for local work rules. Run `uv run --locked pytest` for the generated technical suite. CI checks source without a secret on PRs; a private SDK build requires a reviewed SHA and an approved, narrowly scoped App installation.

## Tested foundation commands

The shared template owns `Makefile`, `tests/foundation/` and `deploy/chart/`. Project-owned tests,
settings, scopes and migration history survive updates. `make test-unit` reports isolated
coverage; `make test` also measures enabled integration before advisory report generation. Missing owned
paths remain visible in the report. `make build image verify` uses the canonical wheel
container graph and requires the separately approved SDK-only BuildKit secret.

`make chart-check chart` validates/packages the shared chart. `make deploy-local` and
`make undeploy-local` operate only the explicit disposable Kind contour. External deployment
requires an approved destination and tested image manifest digest. Private runtime CI requires
a full reviewed SHA and the SDK-only App approval gate; skipped runtime checks do not pass.

## Docker verification

`make source` parses the required frozen lock, checks formatting/lint and compiles source using
public Docker tools without installing the SDK. It does not verify runtime types or tests.
After approved SDK access, `make check build verify image` uses the canonical Docker graph;
provide the short-lived token only with `DOCKER_BUILD_ARGS='--secret id=sdk_token,env=SDK_APP_TOKEN'`.
`check` runs strict static/unit checks once; `verify` freshly tests the installed wheel and any
enabled disposable PostgreSQL profile, with separate `coverage/installed/` reports. `make coverage`
reads current source reports without rerunning tests. Below 100% warns; assertions, types,
build/configuration/access and invalid/stale report errors fail.

Private SDK acceptance follows the [reviewed-source admission guide](.github/docs/sdk-admission.md).
The trusted default-branch workflow uses a separately admitted full source SHA; integrated
commits do not need an open PR. This is the approved solo-owner review model, not independent
environment approval. Ordinary source checks do not establish runtime acceptance.
## Observability and license

`GET /metrics` is an internal scrape endpoint, excluded from public gateway routes.
The factory owns a separate registry with bounded route/method/status metrics and
process CPU/RSS. `make run` and the runtime image use `app_logging.json`: JSON logs
carry validated request correlation and safe exception classes, without raw query,
bodies, credentials or traceback text. Run one worker per container; scale replicas.

Project code uses [MIT](LICENSE). Preserve the scaffold notice and upstream terms
in [third-party notices](THIRD_PARTY_NOTICES.md).
