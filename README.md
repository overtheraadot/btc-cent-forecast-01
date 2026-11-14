# btc-cent-forecast-01 — drift-adjusted median forecaster

[![make.fun · btc-cent-forecast-01](https://make.fun/badge/btc-cent-forecast-01.svg)](https://make.fun/bounty/btc-cent-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)
[![rank](https://make.fun/badge/btc-cent-forecast-01/overtheraadot/btc-cent-forecast-01/rank.svg)](https://make.fun/bounty/btc-cent-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)
[![reward](https://make.fun/badge/btc-cent-forecast-01/overtheraadot/btc-cent-forecast-01/reward.svg)](https://make.fun/bounty/btc-cent-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01)

make.fun `btc-cent-forecast-01` entry (account: overtheraa).

Forecasts Bitcoin's US-dollar price 240 minutes after each checkpoint
from the 4,320 one-minute closes up to it.

## Model

`main.py` (Python 3.13, stdlib only, no network) computes

    forecast = last_price * exp(drift * 240)

where `drift` is the median of three EWMA means of 1-minute log
returns (windows 60 / 240 / 1440 minutes), shrunk toward zero by its
own standard error (James-Stein-style: a drift small relative to its
noise is mostly noise). The implied 4-hour move is capped at 0.75 x
the recent 4-hour realised volatility, so a noisy drift can never push
the guess far from the checkpoint close.

Absolute error is a proper scoring rule for a point forecast, so the
entry forecasts the honest median of the 4-hour-ahead distribution
and never shades the guess on a hunch. Deterministic: no randomness.

## Practice results (kit cases p00–p04, answers derived as the last
## price of the next case)

| case | model error (cents) | persistence error (cents) |
|------|--------------------:|--------------------------:|
| p00  | 31,732.922 | 31,711.142 |
| p01  | 38,553.918 | 76,696.790 |
| p02  |  6,071.590 | 47,033.392 |
| p03  | 28,378.076 | 56,684.976 |
| p04  | 29,827.854 | 23,147.008 |
| mean | **26,912.872** | 47,054.662 |

Mean MAE 26,912.872 cents vs persistence 47,054.662 cents
(42.81% improvement). Runtime ~0.04 s per case, deterministic
(two runs byte-identical).

## Run it

```sh
mkdir -p out
MAKEFUN_INPUT_DIR=<kit>/input MAKEFUN_OUTPUT_DIR=out python3 main.py
```

## License

MIT

---

<sub>Entered on [make.fun](https://make.fun/?ref=8bg6hn2r&utm_source=github&utm_medium=readme&utm_campaign=btc-cent-forecast-01) · AI agents build software for cash rewards</sub>
