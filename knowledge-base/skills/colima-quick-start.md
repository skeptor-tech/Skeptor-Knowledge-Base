# Colima Quick Start on macOS

## Tags

- devops
- containers
- macos
- virtualization

Colima gives macOS users a lightweight Linux virtual machine that runs Docker
and containerd workloads without the resource overhead of Docker Desktop. The
commands below focus on the shortest path from installation to a ready-to-use
environment that mirrors common production defaults.

## Prerequisites

- macOS 13+ on Apple Silicon or Intel; Apple Silicon benefits from the `vz`
  virtualization framework.
- Homebrew (`brew --version`) for installing Colima and dependencies.
- Docker CLI (`brew install docker`) or NerdCTL if you prefer containerd.

## Install Colima

```bash
brew install colima lima
```

Homebrew drops the `colima` binary in `$(brew --prefix)/bin`. Reinstalling after
major macOS updates helps keep the Lima base image current.

## One-command start

```bash
colima start --vm-type=vz --arch=aarch64 --cpu 4 --memory 6 --disk 60
```

- `--vm-type=vz` activates Apple's Virtualization framework (fastest on Apple
  Silicon); omit for Intel chips.
- `--arch=aarch64` keeps images compatible with most arm64 container builds; use
  `x86_64` if you routinely run amd64-only images.
- Adjust CPU/memory/disk knobs to match your workload; Colima hot-resizes on the
  next `colima restart`.

Once the VM boots, the default Docker context switches automatically. Confirm
via `docker info` or `docker run hello-world`.

## Daily workflow

1. **Start/stop** – `colima start` and `colima stop` mirror laptop sessions.
2. **Profiles** – Use `colima start --profile k8s` for separate workloads (e.g.
   a k3s cluster). Switch with `colima default k8s`.
3. **Mounts** – Colima automatically mounts your home directory; add focused
   paths via `colima start --mount ~/code:w`.
4. **Container runtime choice** – Append `--runtime containerd` plus
   `--edit` to toggle between Docker and containerd stacks.

## Useful commands

- `colima list` – Shows profiles, status, and runtime details.
- `colima status --json` – Scriptable health info for CI checks.
- `colima x ssh` – Drops you into the Lima host for manual tuning.
- `colima delete` – Destroys the VM (images remain in `~/Library/Containers`).

## Troubleshooting quick hits

- **Port forwarding fails** – Restart with `colima restart --network-address`
  to assign a consistent IP, then reconfigure proxies.
- **File sharing lag** – Limit mounts to the project directory and add
  `--mount-type=virtiofs` (requires `--vm-type=vz`).
- **Image arch mismatch** – Force `docker buildx build --platform linux/amd64`
  to emulate x86 images, or restart Colima with `--arch=x86_64`.

## See also

- [Colima documentation](https://github.com/abiosoft/colima)
