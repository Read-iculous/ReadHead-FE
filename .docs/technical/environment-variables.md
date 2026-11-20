# Environment Variables — ReadHead-FE

## Configuration Overview
ReadHead-FE utilizes Vite environment variables prefixed with `VITE_`.
Different environments are managed via `.env.development`, `.env.production`, and `.env.test`.

## Variable Specifications

| Variable Name | Required | Default Value | Description |
|---|---|---|---|
| `VITE_API_BASE_URL` | Yes | `http://localhost:8000` | Base URL of ReadHead-BE API |
| `VITE_AI_STREAM_ENDPOINT` | Yes | `/api/v1/ai/stream` | Path to SSE streaming endpoint |
| `VITE_APP_ENV` | Yes | `development` | Target environment (`development`, `staging`, `production`) |
| `VITE_ENABLE_ANALYTICS` | No | `false` | Enable anonymized operational error telemetry |
| `VITE_DOCS_URL` | No | `http://localhost:5173/docs` | URL for external help static documentation |
| `VITE_GOOGLE_OAUTH_CLIENT_ID`| No | `""` | Google OAuth2 client id for extension identity |
| `VITE_GITHUB_OAUTH_CLIENT_ID`| No | `""` | GitHub OAuth2 client id for web portal |

## Production Constraints
- Never bake private backend API keys or master credentials into `VITE_` environment variables.
- All `VITE_` variables are inlined into client JS bundles during build time and are publicly inspectable.
