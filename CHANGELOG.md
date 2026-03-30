# Changelog

## v1.0.1 — No More Label Spaghetti <3

- **chart label collision detection** — TP/SL labels that overlap within 20px now stagger horizontally so you can actually read them
- **dash pattern variety** — each position gets its own dash style (normal, short, dot-dash, long) so overlapping lines stay distinguishable
- **timestamp spacing** — x-axis labels auto-space based on chart width instead of piling on top of each other
- **skip zero timestamps** — no more ghost labels during warmup

## v1.0.0 — Sexy Ass UI Update <3

Initial public release of FoxML Trader — native ImGui GUI with slate theme, TradingView-style charts, real-time Binance websocket data, and zero-dependency ANSI TUI fallback.
