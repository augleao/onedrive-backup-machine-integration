# OneDrive Backup Machine Integration

Home Assistant custom integration that exposes status and controls for a **companion OneDrive Backup Machine API/add-on** in the dashboard.

This repository contains only the HACS integration. It does **not** upload Home Assistant backups to OneDrive by itself.

Current version:
- Integration: 0.2.6
- Minimum Home Assistant: 2025.1.0

## How this differs from the official OneDrive integration

Home Assistant Core ships a built-in [OneDrive](https://www.home-assistant.io/integrations/onedrive/) integration (since 2025.2) that acts as a **backup provider**: it stores Home Assistant backups in your personal OneDrive app folder.

This project is different:

| | Official `onedrive` (Core) | This integration (`onedrive_backup`) |
| --- | --- | --- |
| Purpose | Store HA backups in OneDrive | Dashboard UI for a separate backup-machine companion |
| Works alone? | Yes | No — needs a reachable companion API/add-on |
| Typical use | Automatic HA backup location | Monitor/trigger companion sync/backup jobs |
| Install path | Settings → Devices & services | HACS + companion API/add-on |

If you only need “back up Home Assistant to OneDrive”, use the [official OneDrive integration](https://www.home-assistant.io/integrations/onedrive/). Use this repository only when you already run (or plan to run) the companion OneDrive Backup Machine API/add-on and want sensors/buttons for it in Home Assistant.

## What it provides

- Sensors for latest job status, errors, downloaded count, and skipped count
- A Run Now button entity
- Task-specific Run buttons created from the tasks currently available
- The `onedrive_backup.run_task` service for automations and scripts

## Companion requirement

This integration is a thin HTTP client. After install it only works if a companion OneDrive Backup Machine API (or Supervisor add-on that exposes the same API) is reachable at `addon_url`.

The companion is distributed separately from this repository. Until that companion is publicly available, this integration is useful only as a custom repository for users who already host the API themselves — not as a standalone default-catalog install.

### Expected companion API

The integration calls these endpoints on `addon_url`:

| Method | Path | Purpose |
| --- | --- | --- |
| `GET` | `/api/status` | Companion status |
| `GET` | `/api/tasks` | List of tasks (`tasks` array) |
| `GET` | `/api/jobs` | List of jobs (`jobs` array) |
| `POST` | `/api/backup` | Run default backup/sync |
| `POST` | `/api/tasks/{task_id}/run` | Run a specific task |

Example `configuration.yaml`:

```yaml
onedrive_backup:
  addon_url: http://127.0.0.1:8080
  scan_interval: 30
```

## Installation via HACS

1. Open HACS in Home Assistant.
2. Add this repository as an `Integration` custom repository if it is not yet in the default store.
3. Install the integration.
4. Restart Home Assistant.
5. Ensure the companion API or add-on is running and reachable at the configured `addon_url`.

## Included files

- `custom_components/onedrive_backup`: Home Assistant integration package
- `hacs.json`: HACS metadata
- `.github/workflows`: validation workflows

## Notes

- This repository starts with a fresh Git history.
- No add-on runtime files are included here.
- Brand assets are real PNG files suitable for HACS.
