# RMCP (Rust Remote Management Control Protocol crate)

## Tags

- languages/rust/frameworks
- protocol/mcp

## Overview

`rmcp` is a Rust crate that models the Remote Management Control Protocol layer
that wraps IPMI and ASF remote management traffic. The docs.rs build surfaces
crate-level examples and module pages that explain how to construct, serialize,
and parse RMCP packets when integrating with baseboard management controllers
(BMCs) or remote service processors.

## Key modules and types to explore

- `rmcp::asf` — Presence ping, capability negotiation, and power control
  payloads defined by the Alert Standard Format.
- `rmcp::ipmi` — Structures and enums for IPMI session setup plus OEM payload
  handling helpers when you need to carry IPMI messages inside RMCP frames.
- `rmcp::wire` — Bitflag definitions, serialization helpers, and checksum logic
  to encode and decode packets for transmission over UDP.

## Integration tips

1. Review the crate-level example that crafts an ASF presence ping to understand
   the builder patterns used across the API.
2. Combine `rmcp` with `tokio` or `async-std` UDP sockets to send and receive
   packets; the crate provides strongly typed payloads, while your async runtime
   handles network IO.
3. When targeting specific BMC vendors, check the OEM payload enums to map their
   extensions and ensure your packets set the correct capability flags.

## Resources

- [rmcp on docs.rs](https://docs.rs/rmcp/latest/rmcp/)
- [rmcp crate on crates.io](https://crates.io/crates/rmcp)
