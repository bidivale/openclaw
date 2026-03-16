# summary

When you see update now in the ui - it won't be updated by clicking the update button in ui as its running in docker container. Here is how to update

# Updating OpenClaw (forked + Docker setup)

### Step 1: pull upstream changes into your fork

```bash
git fetch upstream
git checkout main
git merge upstream/main -X ours   # -X ours keeps your changes on conflict
```

`-X ours` is already documented in doc 01 — it keeps your customisations when there's a conflict in the same file.

Push the merge to your fork on GitHub 

```bash
git push origin main
```

### Step 2: rebuild the Docker image from the updated source

`docker compose build` does **nothing** here — there is no `build:` section in `docker-compose.yml`.
The image was built by `docker-setup.sh` using a plain `docker build`, so rebuilding means the same:

```bash
docker build -t openclaw:local .
```

This rebuilds the `openclaw:local` image from your local source.
It will take a few minutes (full build, same as first time).

### Step 3: restart the container with the new image

```bash
docker compose up -d openclaw-gateway
```

Compose sees the image tag `openclaw:local` was updated and restarts the container.
Your `.env`, `config.json`, and volumes are preserved — no re-onboarding needed.

### Step 4: verify

```bash
docker compose exec openclaw-gateway node dist/index.js --version
```

Refresh the Control UI — the version badge in the top-right should show the new version and the update banner should be gone.

---

## Full update command (copy-paste)

Run from inside the project directory:

git fetch upstream
git checkout main
git merge upstream/main -X ours
git push origin main
docker build -t openclaw:local .
docker compose up -d openclaw-gateway




## What is preserved across updates

| Thing                          | Preserved? | Where it lives                  |
| ------------------------------ | ---------- | ------------------------------- |
| Gateway token                  | Yes        | `.env` (OPENCLAW_GATEWAY_TOKEN) |
| AI provider keys               | Yes        | Docker volume / `config.json`   |
| Channel config (Telegram etc.) | Yes        | Docker volume / `config.json`   |
| Paired browser devices         | Yes        | Docker volume                   |
| allowedOrigins config          | Yes        | Docker volume / `config.json`   |
| Your code customisations       | Yes        | Your fork's `main` branch       |

You only need to re-run `./docker-setup.sh` or re-onboard if:

- You wipe the Docker volume, or
- You start fresh on a new machine (see docs 01–05)

---

## Checking when upstream has new releases

```bash
git fetch upstream
git log HEAD..upstream/main --oneline
```

This shows commits in upstream that you haven't merged yet.
Or watch the upstream repo on GitHub for release tags.

---

## Gotcha: config changes between versions

Occasionally upstream adds new config keys or changes defaults.
After a major update, check the gateway logs for warnings:

```bash
docker compose logs --tail=50 openclaw-gateway
```





## Why "Update now" in the UI doesn't work

The Control UI's **Update now** button is designed for native installs (macOS app, global npm).
In your setup, the gateway binary is built from **your fork's source** inside the Docker image.
There is nothing to `npm update` at runtime — the code is baked into the image at build time.
Clicking the button triggers an in-place npm update that silently fails inside the container and the banner never clears.

**Ignore the banner.** Updating means: pull → rebuild → restart.

---
