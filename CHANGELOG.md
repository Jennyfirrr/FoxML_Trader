# Changelog

## v1.0.4 — Why Is It Paused Though <3

- **detailed pause reasons** — header now shows exactly why the gate is closed: price, volume, price+vol, long trend, warmup, cooldown, breaker, volatile, downtrend (no more vague "wait")

## v1.0.3 — Color-Coded Positions <3

- **per-position accent icons** — each position gets a unique shape (circle/square/triangle/diamond) in an accent color so you can visually group entry + TP + SL
- **full-width TP/SL lines** — reverted from 35% for easier price tracing
- **accent-tinted dashed lines** — each position's lines have a slight color shift matching their icon
- **fixed double-counted icon width** — labels no longer overlap in collision groups
- **removed connector brackets** — replaced by the cleaner icon system

## v1.0.2 — Chart Readability Overhaul <3

- **TP/SL lines shortened** — right 35% only, way less visual noise
- **position fade** — older positions at 40% alpha, newest at 100%
- **price in labels** — TP/SL labels now show the dollar amount
- **connector brackets** — thin vertical lines linking TP↔SL per position
- **hover crosshair** — dotted horizontal line + price tag on both price and volume charts
- **Y grid lines** — subtle horizontal lines at each price tick for easy reading
- **manual label rendering** — labels drawn directly on draw list, extend leftward from right edge (no clipping)
- **accumulated width stagger** — colliding labels line up end-to-end based on actual text width

## v1.0.1 — No More Label Spaghetti <3

- **unified label collision** — all chart labels (entry, TP, SL) participate in collision detection, stagger horizontally when within 26px of each other
- **chained grouping** — labels that are close to their neighbor chain into one group (no more missed collisions at group boundaries)
- **dash pattern variety** — each position gets its own dash style (normal, short, dot-dash, long) so overlapping lines stay distinguishable
- **equity chart padding** — 15% Y-axis padding so the P&L curve doesn't slam into edges
- **timestamp fix** — static buffers (was dangling pointer), capped at 5 labels, skip zero timestamps
- **removed private configs** from tracking

## v1.0.0 — Sexy Ass UI Update <3

Initial public release of FoxML Trader — native ImGui GUI with slate theme, TradingView-style charts, real-time Binance websocket data, and zero-dependency ANSI TUI fallback.
