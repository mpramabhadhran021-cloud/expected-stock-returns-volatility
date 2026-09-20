# Expected Stock Returns and Volatility — Replication & Extension

A master's-level replication of **French, K.R., Schwert, G.W. and Stambaugh, R.F. (1987),
"Expected Stock Returns and Volatility," *Journal of Financial Economics* 19, 3–29**, plus a
small, self-contained extension.

## What the paper asks

Is the expected excess return on the stock market related to the market's own volatility? French,
Schwert and Stambaugh (FSS) split monthly stock-market volatility, estimated from daily returns,
into a *predictable* piece and an *unpredictable* piece, and show that the predictable piece has
only a weak, direct relation with the risk premium — but the unpredictable piece has a strong,
reliably negative relation with realized returns, which they interpret as indirect evidence of a
*positive* ex-ante risk–return trade-off.

## Repository contents

```
README.md              -- this file
requirements.txt        -- Python dependencies
data/
  ff_daily.csv           -- daily Fama/French market return (Mkt = Mkt-RF + RF) & risk-free rate
  ff_monthly.csv          -- monthly counterparts
01_replication.ipynb    -- reproduces the paper's main tables/figures on 1928-1984 data
02_extension.ipynb      -- tests whether the same relation holds in 1985-2020 (out of sample)
```

## What each notebook does

**`01_replication.ipynb`**
- Summarizes the paper's research question and methodology.
- Reconstructs monthly volatility from daily returns (eq. 2), decomposes it into predictable /
  unpredictable components with an ARIMA(0,1,3) model (eq. 3–4), and reproduces the paper's Table 1
  (volatility summary statistics & ARIMA coefficients), Figure 1 (realized vs. predicted
  volatility), Table 3 (average risk premiums) and Table 4 (WLS regressions of returns on
  volatility).
- Also reproduces the paper's GARCH and GARCH-in-mean results (Tables 2 & 5) using the `arch`
  package.
- Every results section ends with a side-by-side comparison against the numbers printed in the
  paper, and a closing section explicitly lists what was replicated exactly vs. simplified, and why.

**`02_extension.ipynb`**
- Re-runs the identical methodology on **1985–2020**, a 36-year period the original paper's sample
  could not cover.
- Formally tests (via a pooled regression with an era-interaction term) whether the risk–return
  relation changed after 1984.
- Adds a rolling 15-year window plot of the key coefficients across the full 1928–2020 sample, to
  see whether the relation is a stable feature of the data or specific to one period.
- Re-runs the paper's own leverage-effect robustness check (eq. 13) in both eras.
- Documents a genuine edge case the extension period exposes: the paper's volatility estimator
  (eq. 2) is not guaranteed non-negative, and this actually happens in March 2020.

## Data

The original paper uses the daily S&P 500 and the CRSP NYSE value-weighted return, neither of which
is freely redistributable. This project substitutes the closest freely available equivalent: the
daily and monthly **market factor from Kenneth French's Data Library**
(`Mkt = Mkt-RF + RF`, value-weighted, CRSP-based) and the one-month T-bill rate (`RF`), which is the
same underlying series FSS themselves used for the monthly risk-premium side of their tests.

The CSVs in `data/` were obtained indirectly, via the open-source R package
[`tsrsa`](https://github.com/shabbychef/tsrsa) (S. Pav), whose `data-raw/` scripts simply download
and reformat the official French Data Library files — used here only because the sandbox this
project was built in could not reach `mba.tuck.dartmouth.edu` directly. **If you have normal
internet access**, `01_replication.ipynb` contains a commented-out cell that fetches the same data
directly from the original source via `pandas_datareader`; either source gives (up to trivial
formatting differences) the same numbers.

Coverage: daily data from November 1926, monthly from January 1927, both through December 2020.

## Running this project

```bash
pip install -r requirements.txt
jupyter notebook
```

Run `01_replication.ipynb` first (it documents the methodology used throughout), then
`02_extension.ipynb`. Both notebooks are self-contained and can also be run independently — the
small set of helper functions is redefined at the top of each.

## Summary of findings

- The replication reproduces the paper's Table 1 statistics and ARIMA coefficients to within a few
  thousandths in most cells, and its Table 4/5 risk-premium coefficients with the same sign and
  comparable magnitude/significance throughout — in particular the paper's central number, the
  coefficient on *unpredicted* volatility (≈ −1.0, highly significant in every sample split).
- The extension finds that this same pattern — weak direct evidence, strong indirect evidence — is
  present, essentially unchanged, in the 1985–2020 period, and a formal stability test cannot reject
  the hypothesis that the relation is the same before and after 1984. A full-sample rolling-window
  analysis shows the unpredicted-volatility coefficient has been reliably negative in nearly every
  15-year window since 1928.

See each notebook's final section for the full discussion, including explicit limitations.
