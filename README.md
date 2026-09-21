# notes-app


The app exposes these endpoints:

| Endpoint | Purpose |
|---|---|
| `GET /` | Returns a greeting and the hostname of whatever container served the request |
| `GET /healthz` | Liveness check (no database access) |
| `GET /readyz` | Readiness check (verifies database connectivity) |
| `GET /notes` | List all notes |
| `POST /notes` | Create a note from JSON: `{"body": "..."}` |