# LIQ3 (amihud+kyle+rvol_ratio)

Cross-sectional alpha ensemble for crypto USDT-M perpetual futures.

## Performance (backtest 500d, top-100 QV)

| Metric | Value |
|--------|-------|
| Sharpe | 2.55 |
| Max DD | -27.6% |
| Win Rate | 58.1% |
| Return | +142.6% |

## Factors

| Factor | Transform | Direction |
|--------|-----------|-----------|
| amihud | chg20 | + |
| kyle_lambda | z60 | + |
| rvol_ratio | lvl | - |

## Setup

```bash
pip install ccxt pandas numpy python-dotenv
cp .env.example .env
python run_trader.py
```
