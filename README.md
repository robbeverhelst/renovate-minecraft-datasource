# Renovate Custom Datasource for Minecraft Version Tracking

This repository demonstrates how to use Renovate's custom datasource feature to automatically track and update Minecraft Java Edition versions.

## How It Works

Renovate monitors the official Mojang version manifest API and creates pull requests when new Minecraft versions are available.

### Features

- 📌 **Pinned Versions** - Explicitly control which Minecraft version you're using
- 🔄 **Automated Updates** - Renovate creates PRs when new versions are released
- 🎯 **Flexible Tracking** - Choose between stable releases or bleeding-edge snapshots
- 🔀 **Version Type Switch** - Easy toggle between release and snapshot tracking
- 📊 **Loose Versioning** - Handles both semantic (1.21.6) and snapshot (25w35a) version formats

### Configuration

The setup consists of two parts:

1. **`renovate.json`** - Configures the custom datasource and manager
2. **`versions.yml`** - Contains the tracked Minecraft version

### Data Source

Fetches from Mojang's official version manifest:
- URL: `https://launchermeta.mojang.com/mc/game/version_manifest_v2.json`
- Filters: Based on `minecraft_version_type` setting
  - `"release"`: Stable versions (1.21.6, 1.21.7, etc.)
  - `"snapshot"`: Weekly development builds (25w35a, 25w34b, etc.)
- Format: Returns version IDs appropriate for the selected type

### Usage Examples

#### Track Stable Releases
```yaml
# versions.yml
# renovate: datasource=custom.minecraft versioning=loose
minecraft_version: "1.21.6"
minecraft_version_type: "release"
```

#### Track Snapshots (Weekly Dev Builds)
```yaml
# versions.yml
# renovate: datasource=custom.minecraft versioning=loose
minecraft_version: "25w35a"
minecraft_version_type: "snapshot"
```

When a new version is available, Renovate will:
1. Detect the new version from Mojang's API
2. Filter based on your `minecraft_version_type` setting
3. Create a pull request updating `versions.yml`
4. Include version information in the PR description

### Integration

Use the tracked version in your infrastructure:

```yaml
# docker-compose.yml
services:
  minecraft:
    image: itzg/minecraft-server
    environment:
      VERSION: ${MINECRAFT_VERSION}
```

```typescript
// kubernetes deployment
const minecraftVersion = process.env.MINECRAFT_VERSION || "1.21.4";
```

### Testing Locally

To test this configuration with Renovate locally:

```bash
npx renovate --platform=local --dry-run
```

### API Response Example

The Mojang API returns data like:

```json
{
  "versions": [
    {
      "id": "1.21.8",
      "type": "release",
      "releaseTime": "2025-07-17T12:04:02+00:00"
    },
    {
      "id": "25w35a",
      "type": "snapshot",
      "releaseTime": "2025-08-26T11:51:22+00:00"
    }
  ]
}
```

Our configuration filters to only track `"type": "release"` entries.

### License

MIT

### Contributing

Feel free to submit issues and enhancement requests!