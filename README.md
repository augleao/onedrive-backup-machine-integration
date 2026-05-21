# OneDrive Backup Integration

Home Assistant integration for surfacing OneDrive Backup status and controls in dashboards.

This repository intentionally contains only the HACS integration. It does not include the companion add-on or API service.

Current version:
- Integration: 0.2.5
- Minimum Home Assistant: 2025.1.0

## What It Provides

- Sensors for latest job status, errors, downloaded count, and skipped count.
- A Run Now button entity.
- Task-specific Run buttons created from the tasks currently available.
- The `onedrive_backup.run_task` service for automations and scripts.

## Requirement

This integration expects the companion OneDrive Backup API or add-on to be reachable through `addon_url`.

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

## Included Files

- `custom_components/onedrive_backup`: Home Assistant integration package
- `hacs.json`: HACS metadata
- `.github/workflows`: validation workflows

## Notes

- This repository starts with a fresh Git history.
- No add-on runtime files are included here.
- Brand assets are real PNG files suitable for HACS.
