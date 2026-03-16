# Opening the Control UI from a new machine

After docker setup (docs 02-04), the gateway runs but the browser needs to be authenticated and paired before it can connect. This doc covers the exact steps to do that from any machine.

---

## Context: what doc 04 covers

Doc 04 covers the two commands shown at the end of docker setup:
- `logs -f openclaw-gateway` — to watch the gateway logs
- `gateway health --token "..."` — to verify the gateway is running and healthy

Those are still required first. Do doc 04 before continuing here.

---

## Problem 1: "origin not allowed"

The gateway is bound to LAN (`gateway.bind=lan`), but the default `allowedOrigins` only allows `http://127.0.0.1:18789`. Opening from `localhost` or the LAN IP gets rejected.

### Fix: add your origins to the allowlist

Run this inside the gateway container (replace `192.168.1.41` with your machine's LAN IP):

```bash
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml exec openclaw-gateway \
  node dist/index.js config set gateway.controlUi.allowedOrigins \
  '["http://127.0.0.1:18789","http://localhost:18789","http://192.168.1.41:18789"]'
```

To find your LAN IP:
```bash
hostname -I | awk '{print $1}'
```

Then restart the gateway to apply:
```bash
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml restart openclaw-gateway
```

---

## Problem 2: "gateway token missing"

Opening `http://localhost:18789/` or `/chat?session=main` directly will fail — the gateway requires a token.

### Fix: open with the token URL (first time only)

Use the full URL with the token fragment:

```
http://localhost:18789/#token=<your-token>
```

Your token is shown at the end of the docker setup output (doc 02/03). It looks like:

```
Token: 24a7521875fd9179f133eb4d496e99b7b60d59b01266acb961645ff28df1db53
```

You can also retrieve it anytime:
```bash
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml exec openclaw-gateway \
  node dist/index.js config get gateway.token
```

**Important:** The browser saves the token to local storage after the first load. After that, you can open `http://localhost:18789/` directly — no token in the URL needed.

You only need the `#token=...` URL again if you:
- Clear browser data / cookies
- Use a new browser or incognito
- Switch to a different machine

---

## Problem 3: "pairing required" / disconnected

Even after the token is accepted, the Control UI browser session must be **paired** with the gateway — this is a one-time approval per browser/device.

### Step 1: open the token URL in browser

```
http://localhost:18789/#token=24a7521875fd9179f133eb4d496e99b7b60d59b01266acb961645ff28df1db53
```

Wait a few seconds. The browser sends a pairing request to the gateway.

### Step 2: check for pending pairing requests

```bash
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml exec openclaw-gateway \
  node dist/index.js devices list
```

You will see output like:
```
Pending (1)
┌──────────────────────────────────────┬──────────────────...
│ Request                              │ Device           ...
├──────────────────────────────────────┼──────────────────...
│ e92b6b03-f9e0-473a-bb69-70cdc0b598d9 │ 67d561065acc...  ...
└──────────────────────────────────────┴──────────────────...
```

### Step 3: approve the pairing request

Copy the Request ID from the Pending table and approve it:

```bash
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml exec openclaw-gateway \
  node dist/index.js devices approve <request-id>
```

Example:
```bash
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml exec openclaw-gateway \
  node dist/index.js devices approve e92b6b03-f9e0-473a-bb69-70cdc0b598d9
```

### Step 4: refresh the browser

The browser reconnects automatically after approval. You should see the Control UI load.

---

## After pairing: normal usage

Once paired, just open:
```
http://localhost:18789/
```
or go directly to chat:
```
http://localhost:18789/chat?session=main
```

No token URL needed again (unless you clear browser data or use a new machine).

---

## Gotcha: rate limiting

If you open the UI **without** the token URL multiple times (e.g. navigating to `/chat?session=main` directly before pairing), the gateway rate-limits that IP with:

```
unauthorized: too many failed authentication attempts (retry later)
```

Fix: restart the gateway to clear the rate limit counter:
```bash
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml restart openclaw-gateway
```

Then start from Problem 2 above.

---

## Summary: checklist for a new machine

1. Find your LAN IP: `hostname -I | awk '{print $1}'`
2. Add origins to allowlist (Problem 1 fix above)
3. Restart gateway
4. Open `http://localhost:18789/#token=<your-token>` in browser
5. Run `devices list` → copy the pending Request ID
6. Run `devices approve <request-id>`
7. Refresh browser → done
