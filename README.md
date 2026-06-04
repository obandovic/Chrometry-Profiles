# Pool Magic — Profiles

Public catalog of test-strip **presets** for the Pool Magic app. The app reads
this repo at launch (and on "Check for Updates") so refined swatch colors and new
kits ship without an App Store release.

## Layout

```
manifest.json            # source of truth: which presets exist + versions + hashes
presets/<presetId>.json  # one preset per file
```

### `manifest.json`

```json
{
  "schemaVersion": 1,
  "presets": [
    {
      "presetId": "aquachek-7",
      "version": 2,
      "path": "presets/aquachek-7.json",
      "vendor": "AquaChek",
      "model": "7-Way",
      "sha256": "<lowercase hex sha-256 of the preset file's bytes>"
    }
  ]
}
```

The app verifies each preset file against its `sha256` before using it, so a
tampered file is rejected. The app ships a bundled snapshot and treats this repo
as purely additive — it always works offline.

## Adding or updating a preset

1. Add/edit `presets/<presetId>.json` (see the schema in any existing file —
   Lab swatches per value, ideal ranges, white reference).
2. **Bump `version`** in the file and in its `manifest.json` entry.
3. Recompute the hash and update `sha256` in the manifest:
   ```sh
   shasum -a 256 presets/<presetId>.json
   ```
4. Open a PR. Once merged, connected apps pick up the change on next launch.
   Preset-derived profiles in the app are refreshed in place; user-renamed names
   and scan history are preserved. Custom profiles are never touched.

**`presetId` is permanent** — never rename a published id, only deprecate.
Colors are the durable representation in **CIELAB**; verify new swatches against a
real bottle before publishing (accuracy over convenience).
