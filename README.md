# OneDrive Backup Machine Integration

Home Assistant custom integration that exposes status and controls for the companion **[OneDrive Backup Machine](https://github.com/augleao/onedrive-backup-machine)** Supervisor add-on/API.

This repository contains only the HACS integration. It does **not** upload Home Assistant backups to OneDrive by itself.

Current version:
- Integration: 0.2.7
- Companion add-on: [augleao/onedrive-backup-machine](https://github.com/augleao/onedrive-backup-machine)
- Minimum Home Assistant: 2025.1.0

## How this differs from the official OneDrive integration

Home Assistant Core ships a built-in [OneDrive](https://www.home-assistant.io/integrations/onedrive/) integration (since 2025.2) that acts as a **backup provider**: it stores Home Assistant backups in your personal OneDrive app folder.

This project is different:

| | Official `onedrive` (Core) | This project |
| --- | --- | --- |
| Direction | HA backups → OneDrive | OneDrive files → local disk |
| Purpose | Backup Home Assistant | Local backup machine for cloud files |
| Standalone? | Yes | Needs the companion add-on/API |
| Install path | Settings → Devices & services | Supervisor add-on + HACS integration |

If you only need “back up Home Assistant to OneDrive”, use the [official OneDrive integration](https://www.home-assistant.io/integrations/onedrive/).

## Companion add-on (required)

Install and documentation: https://github.com/augleao/onedrive-backup-machine

1. In Home Assistant: **Settings → Add-ons → Add-on Store → Repositories**
2. Add `https://github.com/augleao/onedrive-backup-machine`
3. Install **OneDrive Backup Machine**, set `client_id`, start it, and complete Microsoft device login in the add-on UI
4. Install this HACS integration and point `addon_url` at the add-on API (default `http://127.0.0.1:8080`)

### Expected companion API

| Method | Path | Purpose |
| --- | --- | --- |
| `GET` | `/api/status` | Companion status |
| `GET` | `/api/tasks` | List of tasks (`tasks` array) |
| `GET` | `/api/jobs` | List of jobs (`jobs` array) |
| `POST` | `/api/backup` | Run default backup/sync |
| `POST` | `/api/tasks/{task_id}/run` | Run a specific task |

## What it provides

- Sensors for latest job status, errors, downloaded count, and skipped count
- A Run Now button entity
- Task-specific Run buttons created from the tasks currently available
- The `onedrive_backup.run_task` service for automations and scripts

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
5. Ensure the companion add-on is running and reachable at the configured `addon_url`.

## Included files

- `custom_components/onedrive_backup`: Home Assistant integration package
- `hacs.json`: HACS metadata
- `.github/workflows`: validation workflows

## Notes

- This repository starts with a fresh Git history.
- Add-on runtime files live in the companion repository, not here.
- Brand assets are real PNG files suitable for HACS.
