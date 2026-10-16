# pokerService boundary

Status: local foundation. Only infrastructure routes are registered; no table, command, player session, or game API exists yet.

## 🧭 Ownership

The authoritative Poker Server role is supported by `virtualMoney/README.md` (which names Poker Server as owner of final stacks) and the reference server in `pokerengine/examples/server/poker_server.py`. A future `pokerService` owns the network session and table orchestration around the public `pokerengine` facade. One table has one authoritative engine instance and a serialized command stream; multiple worker processes cannot independently own the same table. The engine retains rules and state transitions. `virtualMoney` retains the internal ledger, holds, and table escrow. This service must not write its tables directly.

A player receives only authorized public/player views. A trusted engine view and other players' private cards never cross the client boundary. Reconnect, timeout, and command acknowledgement require a separate production protocol decision. The reference newline-delimited JSON/TCP example has no production TLS or authentication and is not this service's transport specification.

## 🔌 Future inputs and outputs

Planned inputs are authenticated player commands and connection lifecycle events; planned outputs are authorized player views and durable result/settlement requests through reviewed contracts. The realtime transport, table ownership across workers, persistence, and idempotency are not selected. No gameplay or money movement is implemented in this foundation.

`pypokerlost` is an HTTP control-plane SDK and does not claim realtime gameplay support. The current installed package starts without a database or outbound credentials. `/health/ready` means this empty process is ready, not that tables can accept players.
