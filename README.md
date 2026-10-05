# Alpha LIQ3 — Liquidity Ensemble

Cross-sectional liquidity factor ensemble for crypto USDT-M perpetual futures.

## Strategy

Short illiquid coins (high amihud, high kyle_lambda) and long liquid ones. Exploits the liquidity premium — illiquid assets compensate holders with higher expected returns, but in crypto futures the effect reverses cross-sectionally.

## Backtest Performance

| Metric | Value |
|--------|-------|
| Period | 500 days (top-100 QV universe) |
| Sharpe | **2.55** |
| Max Drawdown | **-27.6%** |
| Win Rate | 58.1% |
| Total Return | +142.6% |
| Positions | 30 (equal-weight, daily rebalance) |

## Paper Trade Performance

| Factor | PnL | Days | $/Day |
|--------|-----|------|-------|
| `amihud|chg20|+` | $+0.00 | 0 | $+0.000 |
| `kyle|lambda|z60|+` | $+0.00 | 0 | $+0.000 |
| `rvol|ratio|lvl|-` | $+0.00 | 0 | $+0.000 |
| **Total** | **$+0.00** | | |

## Factors (3)

| Factor | Transform | Dir | Description |
|--------|-----------|-----|-------------|
| `amihud` | `chg20` | `+` | 20-day change in Amihud illiquidity ratio |
| `kyle_lambda` | `z60` | `+` | 60-day z-score of Kyle's lambda (price impact per unit volume) |
| `rvol_ratio` | `lvl` | `-` | Ratio of 5-day to 60-day realized volatility (vol regime) |

## Signal Construction

```
1. Compute raw factor for all symbols
2. Apply transform (level, change, z-score)
3. Filter to top-100 by quote volume
4. Cross-sectional z-score (demean + normalize)
5. Winsorize at +/-3 sigma
6. Equal-weight top-30 by |signal|
7. Daily rebalance after candle close (00:00 UTC)
```

## Risk Management

| Parameter | Value |
|-----------|-------|
| Free margin | >= 33% |
| Leverage | 5x |
| Funding filter | Skip if \|rate\| > 0.03% |
| Stop loss | -30% from peak |
| Candle check | Only trade after close |

## Setup

```bash
pip install ccxt pandas numpy python-dotenv
cp .env.example .env
# Add Binance Futures API key + secret
python run_trader.py  # dry run first
# Set LIVE_DRY_RUN=0 to go live
```
