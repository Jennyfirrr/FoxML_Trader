# FoxML Trader

Tick-level crypto trading engine in C++. Branchless fixed-point arithmetic, bitmap-based portfolio management, regime detection with EMA/SMA crossover, and ML inference harness for XGBoost/LightGBM.

Built from scratch as a learning project — no frameworks, no black boxes.

## Features

- **Tick-level execution**: every market tick processed in <100ns on hot path
- **Fixed-point arithmetic**: deterministic 4096-bit FPN — no floating-point rounding variance
- **Branchless hot path**: mask-select patterns eliminate branch misprediction
- **Regime detection**: EMA/SMA crossover with score-based classification (RANGING / TRENDING / VOLATILE)
- **4 strategies**: Mean Reversion, Momentum, SimpleDip, ML (model-driven)
- **ML inference harness**: XGBoost / LightGBM C API integration (~1-5us per prediction)
- **Risk infrastructure**: sticky kill switch, vol-scaled sizing, no-trade band, per-strategy P&L attribution
- **Native GUI**: Dear ImGui + implot (SDL2/OpenGL3) — dockable panels, candlestick charts, live settings editor
- **ANSI TUI**: zero-dependency terminal dashboard with diff-based rendering
- **Live trading**: Binance websocket (market data) + REST API (orders)
- **Paper trading**: full simulation with configurable fees, slippage, and balance

## Quick Start

```bash
# clone and build (ANSI TUI, only needs OpenSSL)
git clone https://github.com/Jennyfirrr/FoxML_Trader.git
cd FoxML_Trader
cmake -B build && cmake --build build

# configure
cp engine.cfg.example engine.cfg    # edit with your settings

# run
cd build && ./engine

# run tests (166 assertions)
./build/controller_test
```

### GUI Build (SDL2 + OpenGL3)

```bash
# install deps (Arch: sdl2, Ubuntu: libsdl2-dev)
cmake -B build_gui -DUSE_IMGUI_GUI=ON
cmake --build build_gui
cd build_gui && ./engine_gui
```

### ML Inference Build

```bash
# XGBoost
cmake -B build -DUSE_XGBOOST=ON && cmake --build build

# LightGBM
cmake -B build -DUSE_LIGHTGBM=ON && cmake --build build
```

## Architecture

```
HOT PATH (every tick, <100ns):
  BuyGate (branchless) -> OrderPool
  PositionExitGate (branchless bitmap walk) -> ExitBuffer

SLOW PATH (every N ticks):
  RollingStats -> RegimeDetector -> Strategy dispatch
  ML inference (if enabled) -> model_score enrichment
  Portfolio P&L -> adaptive gate adjustment
```

## ML Inference Harness

The engine supports model-driven trading via XGBoost or LightGBM:

- **Mode A — Regime Enrichment**: model prediction feeds into `RegimeSignals.model_score`, improving regime classification for all strategies
- **Mode B — ML Strategy**: `STRATEGY_ML` (id=3) uses model predictions directly for buy signal generation

Models are trained offline (Python), engine does inference only. Feature packing maps RegimeSignals + RollingStats fields to a float vector matching the training pipeline.

```cfg
ml_backend=1                    # 1=xgboost, 2=lightgbm
ml_model_path=models/buy.xgb
ml_buy_threshold=0.60           # prediction > 0.6 = buy signal
```

> **Note**: ML harness is wired and compiles clean, but is untested with real models. Inference paths are no-ops when `ml_backend=0` (default).

## Configuration

Copy `engine.cfg.example` to `engine.cfg` and edit. All parameters are hot-reloadable — press `r` in the TUI or edit the file while running.

Key sections: Trading, Entry Filters, Risk Management, Kill Switch, Vol Sizing, No-Trade Band, Regime Detection, ML Inference, Session Filters.

The GUI settings panel exposes all ~68 config fields with hover tooltips.

## Risk Infrastructure

- **Sticky kill switch**: daily loss or drawdown limit breach halts all buying until session reset or manual `k` key. Persists across crashes (snapshot v9).
- **Vol-scaled sizing**: position quantity scales inversely with volatility (consistent risk per trade)
- **No-trade band**: suppresses entries when signal strength < fee breakeven (prevents churn)
- **Per-strategy P&L**: tracks wins/losses/P&L per strategy for comparison

## Project Structure

```
CoreFrameworks/   - OrderGates, Portfolio (bitmap), PortfolioController (feedback loop)
Strategies/       - RegimeDetector, MeanReversion, Momentum, SimpleDip, MLStrategy
ML_Headers/       - RollingStats, ModelInference (XGBoost/LightGBM), WelfordStats
DataStream/       - BinanceCrypto (websocket), EngineTUI (snapshot), TUIAnsi (renderer)
FixedPoint/       - FPN arbitrary-width fixed-point arithmetic library
GUI/              - Dear ImGui panels (dashboard, chart, settings, trade history, log)
tests/            - controller_test.cpp (166 assertions)
```

## Platform Support

- **Linux**: fully tested (Arch, Ubuntu)
- **Windows**: use WSL2 (native Windows not supported)
- **macOS**: should work, untested — install deps via Homebrew

## Build Options

| Flag | Description |
|------|-------------|
| `-DUSE_IMGUI_GUI=ON` | Build native GUI (requires SDL2 + OpenGL) |
| `-DUSE_XGBOOST=ON` | Link XGBoost C API for ML inference |
| `-DUSE_LIGHTGBM=ON` | Link LightGBM C API for ML inference |
| `-DLATENCY_PROFILING=ON` | Enable RDTSCP latency profiling |
| `-DBUSY_POLL=ON` | Spin-poll instead of sleeping (lower latency, higher CPU) |

## TUI Controls

| Key | Action |
|-----|--------|
| `q` | Quit |
| `p` | Pause/unpause buying |
| `r` | Hot-reload config from engine.cfg |
| `s` | Cycle regime (manual override) |
| `k` | Reset kill switch |
| `l` | Cycle TUI layout |

## License

MIT — see [LICENSE](LICENSE)
