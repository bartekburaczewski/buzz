# Deploy on ai-assistant (Coolify)

This fork exists only to hold `deploy/coolify/docker-compose.yaml` on the `deploy` branch.
No source changes vs upstream.

- Coolify Application, build pack `dockercompose`, base directory `/deploy/coolify`,
  compose file `docker-compose.yaml` (Coolify default), branch `deploy`. No domain, no Traefik.
- Relay is published only on the Tailscale IP: `ws://100.105.137.33:3000`.
- Image is pinned (`ghcr.io/block/buzz:sha-<7>`). Update = bump the tag in
  `docker-compose.yaml`, commit, push to `deploy`; Coolify redeploys.
- Secrets are Coolify magic variables (`SERVICE_PASSWORD_64_*`, `SERVICE_HEX_64_*`,
  `SERVICE_USER_*`), generated once by Coolify. Do not rotate
  `SERVICE_HEX_64_RELAYKEY` - it is the relay's signing identity.
- Closed relay: `RELAY_OWNER_PUBKEY` (owner public key, hex) is set as a Coolify
  env var on the app, not in this repo. New members
  (agents, people) are added with `buzz-admin add-member --pubkey <hex>` run
  inside the relay container.
- Sync with upstream: `git fetch upstream && git merge upstream/main` (or a commit
  matching the pinned image sha), then push.
