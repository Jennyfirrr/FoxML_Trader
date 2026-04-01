# Changelog

## v1.1.1 — Clean Code, Fast Cache <3

- **centralized state mutations** — `KillSwitch_Activate/Reset`, `Buying_Halt`, `RecordExit` replace 14 scattered inline sites with single-site functions
- **RecordExit fixes** — time-exit and session-close paths now properly track daily P&L, strategy attribution, Welford stats, and gross wins/losses (were missing before)
- **cache-optimal struct layout** — PortfolioController hot fields packed into 4 cache lines (256 bytes), moved from offset 10,672 to offset 88. RollingStats computed outputs moved from offset 6,664 to offset 8.
- **128-bit FPN default** — native `__uint128_t` now default build, 245 test assertions passing

## v1.1.0 — Safety Audit <3

- **5 P&L bugs fixed** — balance drift, fee double-counting, snapshot persistence
- **safety invariants** — SL floor enforcement, FPN division guards, FPN-only accounting
- **snapshot v10** — entry_time, session stats, kill switch state, strategy attribution
- **245 assertions** — up from 166, covering all safety-critical paths
- **p99 latency display** — all latency panels show percentiles

## v1.0.8 — Toggle Everything <3

- **overlay toggles** — checkboxes in the chart header to show/hide: Ribbon, Sessions, H/L, Tag
- **spread & crosshair** also toggleable via code (all on by default)

## v1.0.7 — Chart Love <3

- **EMA/SMA shaded ribbon** — green fill when EMA > SMA (uptrend), red when below — see the crossover state at a glance
- **live price tag** — colored box on the right Y-axis tracking current price, green/red by direction
- **EMA-SMA spread readout** — header shows `spread: ±Xσ` with color so you know how close the crossover is to flipping
- **session high/low markers** — dim dotted lines marking the session range
- **session dividers** — faint vertical lines at UTC hour boundaries with ASIA/EU/US/OVER labels
- **dropdown labels** — interval and candle count selectors now labeled properly ("60 bars" etc)

## v1.0.6 — Tell Me Why <3

- **detailed gate reasons** — header shows exactly why buying is paused: price, volume, price+vol, long trend, warmup, cooldown, breaker, volatile, downtrend
- **settings tooltips** — all config fields now have hover tooltips, strategy selector shows all 5 options

## v1.0.5 — Drag Your TP/SL Like a Boss <3

- **draggable TP/SL** — hover near a TP/SL line, cursor changes, click and drag to adjust live with real-time P&L preview, writes to engine on release
- **P&L on labels** — TP/SL labels show +/- dollar profit instead of absolute price
- **equity chart crosshair** — hover crosshair with P&L readout tag
- **trade markers fixed** — now match most recent candle instead of oldest

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
