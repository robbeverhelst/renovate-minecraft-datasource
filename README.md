# 🎮 Renovate Minecraft Datasource

![MIT License](https://img.shields.io/badge/License-MIT-blue.svg) ![Renovate](https://img.shields.io/badge/Renovate-enabled-brightgreen.svg)

Custom Renovate datasource for tracking Minecraft Java Edition versions.

## 🚀 Setup

1. **Enable Renovate** for your repository: [Getting Started Guide](https://docs.renovatebot.com/getting-started/installing-onboarding/)

2. **Add configuration:**
   - **Option A:** Copy [`renovate.json`](./renovate.json) to your repository
   - **Option B:** Add the `customManagers` and `customDatasources` from [`renovate.json`](./renovate.json) to your existing renovate.json

3. Add the renovate comment to any file where you want to track Minecraft versions

### Examples

Track releases in docker-compose.yml:
```yaml
services:
  minecraft:
    image: itzg/minecraft-server
    environment:
      # renovate: datasource=custom.minecraft-release
      VERSION: "1.21.8"
```

Track snapshots in a versions file:
```yaml
# renovate: datasource=custom.minecraft-snapshot
minecraft_snapshot: "25w35a"
```

Track in Dockerfile:
```dockerfile
# renovate: datasource=custom.minecraft-release
ENV MINECRAFT_VERSION=1.21.8
```

## 📡 Data Source

- **API**: `https://launchermeta.mojang.com/mc/game/version_manifest_v2.json`
- **Release datasource**: `custom.minecraft-release`
- **Snapshot datasource**: `custom.minecraft-snapshot`

## How it Works

1. Renovate fetches versions from Mojang's API
2. Filters by release type (release/snapshot)  
3. Creates PRs when new versions are available
4. Updates your `versions.yml` file

## License

MIT