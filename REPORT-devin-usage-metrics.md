# Report: Devin usage metrics on the deck — 25 Nov 2026

The owner asked for the `devin` seat's usage metrics on the comms-deck
chat screen. This is what was found, built, and verified.

## The problem

Every other terminal seat feeds the panel's USAGE column somehow: Claude
Code and Grok pipe session JSON into `comms-deck statusline`, Gravity's
model comes from her hooks, and the Grok Bot account is scraped from the
app. Devin CLI has **no status line** and its lifecycle hooks
(SessionStart, UserPromptSubmit, Stop, SessionEnd) carry no usage figures
in their payloads — so `usage/devin.json` could never be written and the
panel showed nothing for the seat.

## What Devin CLI does offer

Devin CLI can export **OTLP metrics** (OpenTelemetry) to any HTTP
endpoint, configured in `~/.config/devin/config.json` under `otel`. The
documented metrics include `devin.token.usage` (tokens by model and
type), `devin.session.count`, `devin.active_time.total`, lines-of-code,
commit and pull-request counters. Export is opt-in, defaults to
OTLP/HTTP **protobuf** on a 60-second interval, and only the metrics
signal needs a destination — so no prompts or tool text ever leave the
machine.

## What was built

comms-deck PR
[#56](https://github.com/whyyyyyai/comms-deck/pull/56) (branch
`devin/usage-otel`, self-test `all passed`):

- `comms-deck serve` now runs a small HTTP listener on
  `127.0.0.1:4318` (`COMMS_DECK_OTEL_PORT` to change) for
  `POST /v1/metrics`. It parses both OTLP/JSON and the default protobuf
  with a small wire-format reader — standard library only, no new
  dependencies. `/v1/logs` and `/v1/traces` are accepted and dropped.
- Each data point is folded into running totals kept in
  `usage/otel/<seat>.json`, so a service restart loses nothing. Delta
  sums add each export; cumulative sums add only the growth; gauges keep
  their latest value. The seat comes from an `X-Comms-Deck-Seat` header
  (default `devin`).
- `usage/devin.json` is written in the panel's usual shape: model name,
  session input/output tokens, and every counter under `otel.metrics`.
- The panel's Session line shows `devin.active_time.total` and
  `devin.session.count` next to the token figures.
- `comms-deck install` adds `otel.metrics_endpoint` and the seat header
  to `~/.config/devin/config.json` using `setdefault` — an endpoint the
  owner chose is never replaced. Only the metrics signal is configured.

## Verified

- `comms-deck self-test`: all checks pass, including a new one that
  posts OTLP/JSON to the real listener and hand-built protobuf directly,
  covering delta vs cumulative sums, gauges, the accumulator file, and
  the rendered panel row.
- End to end: real `devin -p` sessions were run with the endpoint
  pointed at the listener. Protobuf exports arrived and were recorded
  (`devin.session.count`, `devin.active_time.total`).

## Caveats

- `devin.token.usage` is documented but was **not observed** in CLI
  3000.14.30821 during testing — the two counters above are what this
  build actually exports. The code handles token usage whenever the CLI
  emits it, and the OTLP logs signal was checked too: its `api_request`
  events carry no token attributes in this build.
- Takes effect for Devin on its next launch after `install` runs;
  metrics arrive within `metric_export_interval_ms` (default 60s).
- Only the owner merges; the PR awaits a review from another seat.

Written by: Devin
