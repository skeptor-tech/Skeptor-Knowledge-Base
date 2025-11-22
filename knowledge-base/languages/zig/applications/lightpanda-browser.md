# Lightpanda Browser

## Tags

- languages/zig
- languages/zig/applications
- automation/web
- code

## Overview

Lightpanda is a Zig-based headless browser that implements enough of the Chrome
DevTools Protocol (CDP) to act as a drop-in target for Puppeteer, Playwright,
and chromedp clients. The project favors ultra-low resource consumption (roughly
9× less memory than Chrome in the team’s AWS m5.large benchmarks), instant
startup, and predictable execution speeds for AI agents, LLM data collection,
and scraping workloads that need JavaScript execution without a GUI surface.

## Key capabilities

- **CDP server**: `lightpanda serve` exposes a WebSocket endpoint (default
  `ws://127.0.0.1:9222`) that automation clients connect to via
  `browserWSEndpoint`, so existing scripts need minimal changes.
- **Deterministic fetcher**: `lightpanda fetch --dump <url>` streams HTTP logs
  and the final DOM, making it easy to debug network access without running a
  separate automation harness.
- **Playwright/Puppeteer compatibility layer**: Works with current API
  expectations; when Lightpanda adds new Web APIs, Playwright may select new
  execution paths, so pin working versions and report regressions upstream.
- **Multi-platform delivery**: Nightly binaries exist for Linux x86_64 and
  macOS arm64, plus official Docker images (`lightpanda/browser:nightly`) that
  publish the CDP server on port 9222.
- **Performance profile**: Purpose-built in Zig with no Chromium/WebKit
  dependency, relying on Netsurf libraries for HTML/DOM and V8 for JavaScript to
  keep the footprint small.

## Usage notes

- Install via nightly binaries, Docker, or build from source; mark the downloaded
  binary executable (`chmod a+x ./lightpanda`) before use.
- When running inside WSL2, follow the Linux install steps and keep automation
  clients (Puppeteer) on the Windows host for best UX.
- Use Docker for reproducible deployments:

  ```bash
  docker run -d --name lightpanda -p 9222:9222 lightpanda/browser:nightly
  ```

- After `lightpanda serve`, reuse existing automation scripts by pointing them
  to the CDP socket:

  ```javascript
  const browser = await puppeteer.connect({
    browserWSEndpoint: "ws://127.0.0.1:9222",
  });
  ```

- Lightpanda does not render graphics, so plan to inspect DOM/log output or rely
  on automation assertions rather than screenshots.

## Architecture and dependencies

- Uses Netsurf libraries for HTML parsing and DOM tree generation.
- Ships with Mimalloc as the C allocator; dev builds can emit stats with
  `MIMALLOC_SHOW_STATS=1`.
- Vendors Google V8 via `make get-v8` and `make build-v8`, which is a lengthy,
  CPU-heavy compilation step.
- Offers helper targets such as `make install-netsurf` and
  `make install-mimalloc` (with `-dev` variants) to set up dependencies once.

## Resources

- GitHub: [lightpanda-io/browser](https://github.com/lightpanda-io/browser)
- Nightly builds: [Releases › nightly](https://github.com/lightpanda-io/browser/releases/tag/nightly)
- Docker Hub: [lightpanda/browser](https://hub.docker.com/r/lightpanda/browser)
- Benchmarks & demos: [lightpanda-io/demo](https://github.com/lightpanda-io/demo)
- WPT subset: [lightpanda-io/wpt](https://github.com/lightpanda-io/wpt)
