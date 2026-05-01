# hermes-dashboard (Dokploy-ready)

Two-service deployment of [Hermes Agent](https://github.com/NousResearch/hermes-agent), both running from the same `nousresearch/hermes-agent:latest` image:

- **`gateway`** — `hermes gateway run` on `:8642` (internal API).
- **`dashboard`** — `hermes dashboard --host 0.0.0.0 --insecure` on `:9119`, fronted by Traefik on `${HERMES_DOMAIN}`.

Both share `/opt/data` so the dashboard sees the gateway's config, auth, and sessions.

## Deploy via Dokploy

1. Push this directory to a git repo Dokploy can read (or paste `docker-compose.yml` directly into a Compose project).
2. Create a **Compose** service in Dokploy pointing at the repo.
3. Set env vars in the Dokploy UI:

   | Variable | Purpose |
   |---|---|
   | `HERMES_DOMAIN` | Public hostname Traefik routes to the dashboard (no scheme/port). E.g. `hermes.example.com`. |
   | `HERMES_IMAGE` | Optional pin, default `nousresearch/hermes-agent:latest`. |

4. Attach the Traefik domain (`HERMES_DOMAIN`) in Dokploy. Traefik terminates TLS in front of port 9119.
5. Deploy.

## Configuring inference providers

API keys for OpenRouter / Anthropic / OpenAI / Google are configured **through the dashboard UI** — written to the shared `/opt/data` volume. No env vars needed.

## Direct hermes CLI

```bash
docker exec -it hermes-gateway hermes
```

## Persistent volume

`../files/hermes` → `/opt/data` — config, auth, sessions. Bind-mounted to Dokploy's managed `../files/` dir so it survives redeploys. Shared between both services.

## Layout

```
.
├── docker-compose.yml  # gateway + dashboard, both from nousresearch/hermes-agent
├── .env.example        # documents env vars
└── README.md
```
