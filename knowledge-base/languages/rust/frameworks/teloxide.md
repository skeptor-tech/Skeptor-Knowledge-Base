# Teloxide (Rust Telegram Bot Framework)

## Tags

- languages/rust
- languages/rust/frameworks
- messaging/telegram
- code

## Overview

Teloxide is an async-first Rust framework for building Telegram bots on top of
the official Bot API. It wraps Telegram updates in strongly typed structs, wires
in `tokio` runtimes, and ships batteries-included dispatching so you can focus
on bot logic rather than polling loops, rate limits, or manual HTTP plumbing.
The project is dual-licensed (MIT/Apache-2.0) and maintained under
[github.com/teloxide/teloxide](https://github.com/teloxide/teloxide).

## Core capabilities

- **Requester abstraction**: `Bot` implements the `Requester` trait, giving a
  fluent interface (`bot.send_message(...).await?`) with automatic JSON
  serialization and retry helpers.
- **Dispatcher & state machine**: `Dispatcher::builder` plus the `dptree` router
  lets you compose filters, shared state, throttling, and dialogue storage, so
  multi-step conversations stay type-safe.
- **Update adapters**: Supports long polling (`repl`, `Dispatcher::setup_ctrlc`)
  and webhooks (integration with `axum`, `warp`, or custom hyper handlers).
- **Feature-gated crates**: Optional crates (`teloxide-macros`, `teloxide-core`,
  `teloxide-signals`) enable derive macros, granular control, or graceful
  shutdown to keep binary sizes lean.

## Getting started

1. Add the framework and enable macros when you need derive helpers:

   ```bash
   cargo add teloxide --features macros,ctrlc-handler
   ```

2. Bootstrap a bot with environment-based tokens and `repl` for quick
   prototyping:

   ```rust
   use teloxide::prelude::*;

   #[tokio::main]
   async fn main() {
       teloxide::enable_logging!();
       let bot = Bot::from_env().auto_send();

       teloxide::repl(bot, |message: Message, bot: AutoSend<Bot>| async move {
           bot.send_message(message.chat.id, "Hello from teloxide!").await?;
           respond(())
       })
       .await;
   }
   ```

3. For production, swap `repl` with an explicit `Dispatcher` and configure either
   long polling or webhook delivery based on hosting constraints.

## Development patterns

- Store per-chat state via `Storage` backends (in-memory, Redis, SQL) to drive
  guided flows; combine with `Dialogue::get()` to resume sessions.
- Use `UpdateFilterExt` helpers (e.g., `.filter_command::<String>()`) to route
  commands cleanly and reduce boilerplate parsing.
- Leverage middlewares for logging, rate limiting, or instrumentation—`dptree`
  handlers can push spans or metrics to `tracing`.
- When compiling to AWS Lambda/FaaS, enable the `webhooks` feature, disable
  default TLS backends you don't need, and reuse global `reqwest` clients.

## Deployment considerations

- Telegram mandates HTTPS for webhooks; pair Teloxide with `axum` + `rustls` or
  terminate TLS at a reverse proxy (Caddy, Nginx) and forward to your bot.
- Respect Telegram rate limits (30 messages/s global, 1 message/s per chat);
  plug in the built-in throttler middleware or wrap `Requester` calls with
  backoff logic.
- Keep the bot token out of binaries—`Bot::from_env` reads `TELOXIDE_TOKEN`, and
  `dotenvy` integration works well for local runs.

## Resources

- GitHub repo: [teloxide/teloxide](https://github.com/teloxide/teloxide)
- API docs: [docs.rs/teloxide](https://docs.rs/teloxide/latest/teloxide/)
- Guide: [GitHub wiki / guide](https://github.com/teloxide/teloxide/wiki)
- Telegram Bot API reference: <https://core.telegram.org/bots/api>
- Example bots: [examples directory](https://github.com/teloxide/teloxide/tree/master/examples)
