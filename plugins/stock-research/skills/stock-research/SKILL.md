---
name: stock-research
description: Use when building stock/equity research, screeners, backtesting, technical or fundamental analysis, or market-data pipelines — Korean markets (KRX, DART 전자공시, 한국투자증권/키움 REST API) and US markets (yfinance, Alpha Vantage), with pandas/pandas-ta. Covers 주식 데이터 수집, 백테스트, 지표 계산, 재무제표.
---

# Stock Research & Backtesting (주식 데이터/분석)

## Overview
Most quant mistakes are **data mistakes, not model mistakes** — look-ahead bias, survivorship bias, and overfitting produce beautiful backtests that lose real money. Get clean point-in-time data and an honest backtest before any strategy logic. This is a research/tooling skill, not investment advice.

## When to Use
- Fetching Korean (KRX/DART/증권사 API) or US market data
- Computing technical indicators, screening, fundamental analysis
- Backtesting a strategy; building a data pipeline/dashboard

## Data sources
| Need | Source |
|------|--------|
| US OHLCV (quick) | `yfinance` (free, delayed) |
| Korean 시세 (quick) | `pykrx`, `FinanceDataReader` |
| Korean 실거래/주문 | 한국투자증권 KIS REST API, 키움 REST (실계좌 필요) |
| Korean 재무/공시 | **DART OpenAPI** (전자공시, 재무제표, 지분) |
| Fundamentals US | Alpha Vantage, FMP, SEC EDGAR |

```python
import FinanceDataReader as fdr
df = fdr.DataReader('005930', '2023-01-01')   # 삼성전자 OHLCV
```

## Indicators & backtesting
- Indicators: `pandas-ta` or `ta` (SMA/EMA/RSI/MACD/BBands). Don't hand-roll unless learning.
- Backtest: `vectorbt` (fast, vectorized) or `backtrader` (event-driven, realistic).
- Always model **fees + slippage + 세금(거래세 0.18%대, 수수료)** — Korea has 증권거래세; ignoring it flatters results.

## The three biases that fake your returns
| Bias | What it is | Fix |
|------|-----------|-----|
| **Look-ahead** | Using data not knowable at decision time | Shift signals; use point-in-time data; no future columns |
| **Survivorship** | Only testing 상장폐지 안 된 종목 | Include delisted tickers in universe |
| **Overfitting** | Tuning params to past noise | Out-of-sample/walk-forward test; keep it simple |

## Fundamental analysis (DART)
- Pull 재무제표 from DART OpenAPI: 매출/영업이익/순이익, 부채비율, ROE, PER/PBR.
- Compare within sector, watch YoY/QoQ trend, cross-check 감사의견.
- 지분공시·대량보유 for ownership signals.

## Common Mistakes
- Backtest uses adjusted-close but live logic uses raw price → inconsistent → invalid.
- No fees/tax/slippage → strategy looks profitable, isn't.
- Survivorship-biased universe → inflated returns.
- Look-ahead via `df['future_return']` leaking into features.
- Curve-fitting on one period → always keep an untouched out-of-sample window.
- Treating delayed/free data as real-time for a live 실계좌 strategy.
- Confusing research with advice — outputs are analysis, not recommendations; risk is real.
