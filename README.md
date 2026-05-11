# hive-plugin-exchange

A virtual financial exchange plugin for [hive-harness](https://github.com/mchiver/hive-harness).

## Overview

Each Exchange entity represents a distinct market with its own SQLite-backed order book, asset registry, participant management, and automated tick loop. The exchange supports limit order matching by price-time priority, manufacturing production, and consumer liquidation. LLM participants can be wired to conversations for automated trading agents.

## Installation

### Via System.InstallPlugin (Recommended)

From inside a Hive session:

```
System.InstallPlugin { PluginName: "Exchange" }
```

This reads the plugin from the official catalog and copies it into the registry.

### Manual Install

1. Clone this repo into `~/.hives/Plugins/Exchange/` (or wherever your registry lives).
2. Create a `plugin.link.json` in that folder pointing to the cloned path.
3. Restart the Hive.

### Development Setup

The `devDependencies` in `package.json` points to `hive-harness` via a `file:` reference. Run:

```bash
npm install
```

This installs the harness into `node_modules/@mchiver/hive-harness` so `require('@mchiver/hive-harness/...')` resolves correctly for tests and the TUI.

Then run tests:

```bash
npm test
```

## Entities

| Property | Default | Description |
|---|---|---|
| Name | (required) | Exchange entity name. |
| Description | "" | Human-readable description. |
| TickIntervalMs | 10000 | Milliseconds between ticks. |
| LiquidationRate | 0.05 | Fraction of consumer holdings liquidated per tick. |
| StartingEc | 10000 | Default EC balance for new accounts. |
| BaseAssetPrice | 10 | Base price for liquidation when no trade history exists. |

## Tools

| Tool | Description |
|---|---|
| `RegisterAsset` | Register a new asset with supply. |
| `CreateAccount` | Create an account with optional starting EC. |
| `CreateParticipant` | Create a participant (supplier/consumer/speculator/hybrid). |
| `SubmitOrder` | Place a buy or sell limit order. |
| `CancelOrder` | Cancel an open order. |
| `GetOrderBook` | View current bids and asks for an asset. |
| `GetMarketSummary` | View all assets with last price, spread, and volume. |
| `RunTick` | Execute one tick (matching, liquidation, manufacturing). |
| `GetAccount` | View account balance and holdings. |
| `GetHoldings` | List asset holdings for an account. |
| `GetOrders` | List open orders for an account. |
| `GetTrades` | View recent trades for an asset. |
| `GetTradeHistory` | View all trades across all assets. |
| `ListAssets` | List all registered assets. |
| `ListAccounts` | List all accounts. |
| `ListParticipants` | List all participants. |
| `CreditEc` / `DebitEc` | Adjust account EC balances. |
| `CreditHolding` / `DebitHolding` | Adjust account asset holdings. |
| `UpdateParticipant` | Modify participant settings. |
| `RemoveParticipant` | Deactivate a participant. |

## TUI

Run the Blessed-based exchange TUI:

```bash
node Tui/exchange.js --exchange main
```

Key bindings:
- `p` -- start/pause the exchange
- `s` -- step one tick while paused
- `+` / `-` -- increase/decrease tick speed
- `a` -- add a participant
- `e` -- edit selected participant
- `d` -- delete selected participant
- `q` -- quit

## Testing

```bash
# Install dev dependencies (includes hive-harness via file: reference)
npm install

# Run the standalone test suite
npm test
```

## License

MIT
