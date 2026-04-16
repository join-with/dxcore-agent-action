# DxCore Agent Action

Download the DxCore CLI and connect a build agent to the coordinator.

## Inputs

| Input             | Description                                                        | Required | Default  |
|-------------------|--------------------------------------------------------------------|----------|----------|
| `coordinator-url` | URL of the running DxCore coordinator                              | Yes      |          |
| `token`           | Authentication token matching the coordinator                      | Yes      |          |
| `agent-id`        | Unique identifier for this agent. Defaults to matrix value or runner name. | No | `""`     |
| `session-id`      | Session identifier. Defaults to `gha-<run_id>`.                    | No       | `""`     |
| `work-dir`        | Working directory for task execution                               | No       | `.`      |
| `build-system`    | Build system adapter (`turbo` or `nx`)                             | No       | `turbo`  |
| `version`         | Release tag to download (e.g. `v0.1.0`). Defaults to latest.      | No       | `latest` |

## Usage

```yaml
jobs:
  agent:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        index: [0, 1, 2]
    steps:
      - uses: actions/checkout@v4
      - uses: join-with/dxcore-agent-action@main
        with:
          coordinator-url: http://<coordinator-host>:4000
          token: ${{ secrets.DXCORE_TOKEN }}
          build-system: turbo
```

## How It Works

1. Resolves the release version (latest or pinned)
2. Downloads the CLI binary from [`join-with/dxcore`](https://github.com/join-with/dxcore) releases
3. Derives a unique agent ID from the matrix configuration or runner name
4. Waits for the coordinator to become available
5. Connects the agent and begins executing tasks

## Related

- [join-with/dxcore](https://github.com/join-with/dxcore) -- Main DxCore repository
- [join-with/dxcore-coordinator-action](https://github.com/join-with/dxcore-coordinator-action) -- Start coordinator
- [join-with/dxcore-dispatch-action](https://github.com/join-with/dxcore-dispatch-action) -- Dispatch task graphs
- [join-with/dxcore-shutdown-action](https://github.com/join-with/dxcore-shutdown-action) -- Shutdown coordinator

## Disclaimer

DxCore is an independent project by Eyal Lapid ([JoinWith])(https://joinwith.com) and is not affiliated with, endorsed by, or sponsored by Vercel, Nrwl, Gradle, Docker, GitHub, or any other third-party project. All trademarks belong to their respective owners.
