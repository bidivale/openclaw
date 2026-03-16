# during dockersetu I got the following questions in the terminal - with my answers

I understand this is personal-by-default and shared/multi-user use requires lock-down. Continue?
│ Yes
│
◇ Onboarding mode
│ QuickStart
│
◇ QuickStart ─────────────────────────╮
│ │
│ Gateway port: 18789 │
│ Gateway bind: Loopback (127.0.0.1) │
│ Gateway auth: Token (default) │
│ Tailscale exposure: Off │
│ Direct to chat channels. │
│ │
├──────────────────────────────────────╯
│
◇ Model/auth provider
│ Mistral AI
│
◇ How do you want to provide this API key?
│ Paste API key now
│
◇ Enter Mistral API key
│ Qss-------------fL
│
◇ Model configured ──────────────────────────────────╮
│ │
│ Default model set to mistral/mistral-large-latest │
│ │
├─────────────────────────────────────────────────────╯
│
◇ Default model
│ mistral/ministral-8b-latest
│
◇ Channel status ────────────────────────────╮
│ │
│ Telegram: needs token │
│ WhatsApp (default): not linked │
│ Discord: needs token │
│ Slack: needs tokens │
│ Signal: needs setup │
│ signal-cli: missing (signal-cli) │
│ iMessage: needs setup │
│ imsg: missing (imsg) │
│ IRC: not configured │
│ Google Chat: not configured │
│ Feishu: install plugin to enable │
│ Google Chat: install plugin to enable │
│ Nostr: install plugin to enable │
│ Microsoft Teams: install plugin to enable │
│ Mattermost: install plugin to enable │
│ Nextcloud Talk: install plugin to enable │
│ Matrix: install plugin to enable │
│ BlueBubbles: install plugin to enable │
│ LINE: install plugin to enable │
│ Zalo: install plugin to enable │
│ Zalo Personal: install plugin to enable │
│ Synology Chat: install plugin to enable │
│ Tlon: install plugin to enable │
│ │
├─────────────────────────────────────────────╯
│
◇ How channels work ───────────────────────────────────────────────────────────────────────╮
│ │
│ DM security: default is pairing; unknown DMs get a pairing code. │
│ Approve with: openclaw pairing approve <channel> <code> │
│ Public DMs require dmPolicy="open" + allowFrom=["*"]. │
│ Multi-user DMs: run: openclaw config set session.dmScope "per-channel-peer" (or │
│ "per-account-channel-peer" for multi-account channels) to isolate sessions. │
│ Docs: channels/pairing │
│ │
│ Telegram: simplest way to get started — register a bot with @BotFather and get going. │
│ WhatsApp: works with your own number; recommend a separate phone + eSIM. │
│ Discord: very well supported right now. │
│ IRC: classic IRC networks with DM/channel routing and pairing controls. │
│ Google Chat: Google Workspace Chat app with HTTP webhook. │
│ Slack: supported (Socket Mode). │
│ Signal: signal-cli linked device; more setup (David Reagans: "Hop on Discord."). │
│ iMessage: this is still a work in progress. │
│ Feishu: 飞书/Lark enterprise messaging with doc/wiki/drive tools. │
│ Nostr: Decentralized protocol; encrypted DMs via NIP-04. │
│ Microsoft Teams: Bot Framework; enterprise support. │
│ Mattermost: self-hosted Slack-style chat; install the plugin to enable. │
│ Nextcloud Talk: Self-hosted chat via Nextcloud Talk webhook bots. │
│ Matrix: open protocol; install the plugin to enable. │
│ BlueBubbles: iMessage via the BlueBubbles mac app + REST API. │
│ LINE: LINE Messaging API bot for Japan/Taiwan/Thailand markets. │
│ Zalo: Vietnam-focused messaging platform with Bot API. │
│ Zalo Personal: Zalo personal account via QR code login. │
│ Synology Chat: Connect your Synology NAS Chat to OpenClaw with full agent capabilities. │
│ Tlon: decentralized messaging on Urbit; install the plugin to enable. │
│ │
├───────────────────────────────────────────────────────────────────────────────────────────╯
│
◇ Select channel (QuickStart)
│ Telegram (Bot API)
│
◇ Telegram bot token ───────────────────────────────────────────────────────────────────╮
│ │
│ 1) Open Telegram and chat with @BotFather │
│ 2) Run /newbot (or /mybots) │
│ 3) Copy the token (looks like 123456:ABC...) │
│ Tip: you can also set TELEGRAM_BOT_TOKEN in your env. │
│ Docs: https://docs.openclaw.ai/telegram │
│ Website: https://openclaw.ai │
│ │
├────────────────────────────────────────────────────────────────────────────────────────╯
│
◇ How do you want to provide this Telegram bot token?
│ Enter Telegram bot token
│
◇ Enter Telegram bot token
│ 8357677563:AAHnNuyCzPe0TcDOqXK4oAjJ2IsczQHrWXo
│
◇ Selected channels ────────────────────────────────────────────────────────────────────────────────╮
│ │
│ Telegram — simplest way to get started — register a bot with @BotFather and get going. │
│ https://docs.openclaw.ai/channels/telegram │
│ https://openclaw.ai │
│ │
├────────────────────────────────────────────────────────────────────────────────────────────────────╯
Updated ~/.openclaw/openclaw.json
Workspace OK: ~/.openclaw/workspace
Sessions OK: ~/.openclaw/agents/main/sessions
│
◇ Web search ────────────────────────────────────────╮
│ │
│ Web search lets your agent look things up online. │
│ Choose a provider and paste your API key. │
│ Docs: https://docs.openclaw.ai/tools/web │
│ │
├─────────────────────────────────────────────────────╯
│
◇ Search provider
│ Skip for now
│
◇ Skills status ─────────────╮
│ │
│ Eligible: 3 │
│ Missing requirements: 41 │
│ Unsupported on this OS: 7 │
│ Blocked by allowlist: 0 │
│ │
├─────────────────────────────╯
│
◇ Configure skills now? (recommended)
│ Yes
│
◇ Install missing skill dependencies
│ Skip for now
│
◇ Set GOOGLE_PLACES_API_KEY for goplaces?
│ No
│
◇ Set GEMINI_API_KEY for nano-banana-pro?
│ No
│
◇ Set NOTION_API_KEY for notion?
│ No
│
◇ Set OPENAI_API_KEY for openai-image-gen?
│ No
│
◇ Set OPENAI_API_KEY for openai-whisper-api?
│ No
│
◇ Set ELEVENLABS_API_KEY for sag?
│ No
│
◇ Hooks ──────────────────────────────────────────────────────────────────╮
│ │
│ Hooks let you automate actions when agent commands are issued. │
│ Example: Save session context to memory when you issue /new or /reset. │
│ │
│ Learn more: https://docs.openclaw.ai/automation/hooks │
│ │
├──────────────────────────────────────────────────────────────────────────╯
│
◇ Enable hooks?
│ Skip for now
Config overwrite: /home/node/.openclaw/openclaw.json (sha256 564e8ad28c16aaa1c5523ee9d8e8a757aad95277de0ec3d4756b920d0ecbcbc3 -> 1ee09ecb189828e75a6297a9a6fb2389a59aa5980519d8a42583a05343f35a14, backup=/home/node/.openclaw/openclaw.json.bak)
│
◇ Systemd ───────────────────────────────────────────────────────────────────────────────╮
│ │
│ Systemd user services are unavailable. Skipping lingering checks and service install. │
│ │
├─────────────────────────────────────────────────────────────────────────────────────────╯
│
◇  
Health check failed: gateway closed (1006 abnormal closure (no close frame)): no close reason
Gateway target: ws://127.0.0.1:18789
Source: local loopback
Config: /home/node/.openclaw/openclaw.json
Bind: loopback
│
◇ Health check help ────────────────────────────────╮
│ │
│ Docs: │
│ https://docs.openclaw.ai/gateway/health │
│ https://docs.openclaw.ai/gateway/troubleshooting │
│ │
├────────────────────────────────────────────────────╯
│
◇ Optional apps ────────────────────────╮
│ │
│ Add nodes for extra features: │
│ - macOS app (system + notifications) │
│ - iOS app (camera/canvas) │
│ - Android app (camera/canvas) │
│ │
├────────────────────────────────────────╯
│
◇ Control UI ─────────────────────────────────────────────────────────────────────────────────────╮
│ │
│ Web UI: http://127.0.0.1:18789/ │
│ Web UI (with token): │
│ http://127.0.0.1:18789/#token=24a7521875fd9179f133eb4d496e99b7b60d59b01266acb961645ff28df1db53 │
│ Gateway WS: ws://127.0.0.1:18789 │
│ Gateway: not detected (gateway closed (1006 abnormal closure (no close frame)): no close │
│ reason) │
│ Docs: https://docs.openclaw.ai/web/control-ui │
│ │
├──────────────────────────────────────────────────────────────────────────────────────────────────╯
│
◇ Workspace backup ────────────────────────────────────────╮
│ │
│ Back up your agent workspace. │
│ Docs: https://docs.openclaw.ai/concepts/agent-workspace │
│ │
├───────────────────────────────────────────────────────────╯
│
◇ Security ──────────────────────────────────────────────────────╮
│ │
│ Running agents on your computer is risky — harden your setup: │
│ https://docs.openclaw.ai/security │
│ │
├─────────────────────────────────────────────────────────────────╯
│
◇ Shell completion ───────────────────────────────────────────────────────╮
│ │
│ Shell completion installed. Restart your shell or run: source ~/.zshrc │
│ │
├──────────────────────────────────────────────────────────────────────────╯
│
◇ Dashboard ready ────────────────────────────────────────────────────────────────────────────────╮
│ │
│ Dashboard link (with token): │
│ http://127.0.0.1:18789/#token=24a7521875fd9179f133eb4d496e99b7b60d59b01266acb961645ff28df1db53 │
│ Copy/paste this URL in a browser on this machine to control OpenClaw. │
│ No GUI detected. Open from your computer: │
│ ssh -N -L 18789:127.0.0.1:18789 user@<host> │
│ Then open: │
│ http://localhost:18789/ │
│ http://localhost:18789/#token=24a7521875fd9179f133eb4d496e99b7b60d59b01266acb961645ff28df1db53 │
│ Docs: │
│ https://docs.openclaw.ai/gateway/remote │
│ https://docs.openclaw.ai/web/control-ui │
│ │
├──────────────────────────────────────────────────────────────────────────────────────────────────╯
│
◇ Web search ───────────────────────────────────────╮
│ │
│ Web search was skipped. You can enable it later: │
│ openclaw configure --section web │
│ │
│ Docs: https://docs.openclaw.ai/tools/web │
│ │
├────────────────────────────────────────────────────╯
│
◇ What now ─────────────────────────────────────────────────────────────╮
│ │
│ What now: https://openclaw.ai/showcase ("What People Are Building"). │
│ │
├──────────────────────────────────

Onboarding complete. Use the dashboard link above to control OpenClaw.

==> Docker gateway defaults
[+] Creating 1/0
✔ Container openclaw-openclaw-gateway-1 Running0.0s
Config overwrite: /home/node/.openclaw/openclaw.json (sha256 1ee09ecb189828e75a6297a9a6fb2389a59aa5980519d8a42583a05343f35a14 -> 96796fae8b94431d0ed2f3ad1362d505668680111dd982eea35a95207e0d78f6, backup=/home/node/.openclaw/openclaw.json.bak)
[+] Creating 1/0
✔ Container openclaw-openclaw-gateway-1 Running0.0s
Config overwrite: /home/node/.openclaw/openclaw.json (sha256 96796fae8b94431d0ed2f3ad1362d505668680111dd982eea35a95207e0d78f6 -> 0bfc2ebe9eb5c1afc14521cdeb8641618616b7a82fdc2bb574ecb3fab731b41c, backup=/home/node/.openclaw/openclaw.json.bak)
Pinned gateway.mode=local and gateway.bind=lan for Docker setup.

==> Control UI origin allowlist
[+] Creating 1/0
✔ Container openclaw-openclaw-gateway-1 Running0.0s
Config overwrite: /home/node/.openclaw/openclaw.json (sha256 b4648996430548f7aacb4542821c9e858bfa53cd11e658f15522559ff670d909 -> ade975ea940b18ca2510cc48a6b61b7ed3d52fdd2f4e62442c35e87b883339ff, backup=/home/node/.openclaw/openclaw.json.bak)
Set gateway.controlUi.allowedOrigins to ["http://127.0.0.1:18789"] for non-loopback bind.

==> Provider setup (optional)
WhatsApp (QR):
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml run --rm openclaw-cli channels login
Telegram (bot token):
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml run --rm openclaw-cli channels add --channel telegram --token <token>
Discord (bot token):
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml run --rm openclaw-cli channels add --channel discord --token <token>
Docs: https://docs.openclaw.ai/channels

==> Starting gateway
[+] Running 1/0
✔ Container openclaw-openclaw-gateway-1 Running 0.0s
[+] Creating 1/0
✔ Container openclaw-openclaw-gateway-1 Running0.0s
Config overwrite: /home/node/.openclaw/openclaw.json (sha256 ade975ea940b18ca2510cc48a6b61b7ed3d52fdd2f4e62442c35e87b883339ff -> 20f0d46b54cabb6ff53ef00f604989da38aa690d1d0968ceb16c3f12c05e343d, backup=/home/node/.openclaw/openclaw.json.bak)

Gateway running with host port mapping.
Access from tailnet devices via the host's tailnet IP.
Config: /home/skmindlab/.openclaw
Workspace: /home/skmindlab/.openclaw/workspace
Token: 24a7521875fd9179f133e-------------645ff28df1db53

Commands 
docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml logs -f openclaw-gateway

docker compose -f /home/skmindlab/proj/mindai-setup/openclaw/docker-compose.yml exec openclaw-gateway node dist/index.js health --token "24a7521875fd9179f133eb4d------------645ff28df1db53"
