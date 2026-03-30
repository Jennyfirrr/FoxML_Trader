# Changelog

## v1.0.1 — No More Label Spaghetti <3

- **unified label collision** — all chart labels (entry, TP, SL) participate in collision detection, stagger horizontally when within 26px of each other
- **chained grouping** — labels that are close to their neighbor chain into one group (no more missed collisions at group boundaries)
- **dash pattern variety** — each position gets its own dash style (normal, short, dot-dash, long) so overlapping lines stay distinguishable
- **equity chart padding** — 15% Y-axis padding so the P&L curve doesn't slam into edges
- **timestamp fix** — static buffers (was dangling pointer), capped at 5 labels, skip zero timestamps
- **removed private configs** from tracking

## v1.0.0 — Sexy Ass UI Update <3

Initial public release of FoxML Trader — native ImGui GUI with slate theme, TradingView-style charts, real-time Binance websocket data, and zero-dependency ANSI TUI fallback.
