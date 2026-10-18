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
coverage; `make test` also measures enabled integration before the full 100% gate. Missing owned
paths remain visible in the report. `make build image test-image` uses the canonical wheel
container graph and requires the separately approved SDK-only BuildKit secret.

`make chart-check chart` validates/packages the shared chart. `make deploy-local` and
`make undeploy-local` operate only the explicit disposable Kind contour. External deployment
requires an approved destination and tested image manifest digest. Private runtime CI requires
a full reviewed SHA and the SDK-only App approval gate; skipped runtime checks do not pass.
