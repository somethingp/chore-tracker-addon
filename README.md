# Chore Tracker Add-on for Home Assistant

Backend service for the [Chore Tracker Card](https://github.com/YOUR_USERNAME/chore-tracker-card). Stores chore state in `/data/chores.json` so all devices stay in sync.

## Features

- REST API for chore state (get, add, delete, complete, undo)
- Persists data across restarts via HA's `/data` volume
- Runs on all HA architectures (amd64, aarch64, armv7, armhf, i386)
- Accessible via HA Ingress — no port forwarding needed

## Installation

This add-on is installed through the **HA Supervisor add-on store** — not HACS.

### 1. Add this repository to the Supervisor

**Settings → Add-ons → Add-on Store → ⋮ (top right) → Repositories**

Paste: `https://github.com/somethingp/chore-tracker-addon`

Click **Add**, then close the dialog.

### 2. Install the add-on

**Chore Tracker** will now appear in the store. Click it → **Install** → **Start**.

Check the **Log** tab and confirm you see:

```
[chore-tracker] Starting on port 8787
```

### 3. Install the companion Lovelace card

Install the **Chore Tracker Card** via HACS Frontend:
👉 [github.com/somethingp/chore-tracker-card](https://github.com/somethingp/chore-tracker-card)

## API reference

The API is available via HA Ingress at `/api/hassio_ingress/chore_tracker`.

| Method | Path | Body | Description |
|--------|------|------|-------------|
| GET | /api/chores | — | Full state |
| POST | /api/chores | `{name, freqDays}` | Add a chore |
| DELETE | /api/chores/:id | — | Delete a chore |
| POST | /api/chores/:id/complete | `{user}` | Mark done |
| POST | /api/chores/:id/undo | — | Undo last completion |
| GET | /api/health | — | Health check |

## Resetting data

SSH into HA and run:
```bash
docker exec $(docker ps -q -f name=chore_tracker) rm /data/chores.json
```
Then restart the add-on to reload defaults.

## License

MIT
