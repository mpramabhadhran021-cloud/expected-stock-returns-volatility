# Expected Stock Returns and Volatility

Replication of French, Schwert and Stambaugh (1987), *Expected Stock Returns and Volatility*, with a small extension.

## Question

The paper asks whether stock-market volatility is related to expected stock returns.

I try to reproduce the main idea of the paper using public Fama/French data.

## Data

The original study used S&P 500 and CRSP data from 1928–1984. I use the Fama/French market return and risk-free rate instead because the original CRSP data are not freely available.

This is therefore a **replication using a close public substitute**, not an exact reproduction.

The data cover 1928–2020.

## Method

The project follows the main steps in the paper:

1. Calculate monthly volatility from daily market returns.
2. Separate volatility into predictable and unpredictable parts using an ARIMA model.
3. Relate excess stock returns to the two volatility measures.
4. Estimate the paper's GARCH models.
5. Compare the main results with the original paper.

## Files

- `01_replication.ipynb` — main replication
- `02_extension.ipynb` — simple test using 1985–2020
- `data/` — data used by the notebooks
- `requirements.txt` — Python packages

## Extension

The second notebook asks whether the relationship looks similar after 1984.

It:
- repeats the main regression for 1985–2020,
- compares the two periods,
- uses a simple interaction test,
- and looks at rolling 15-year windows.

I treat this as an exploratory extension because the data and method have limitations.

## Main limitations

- The market data are a public substitute for the original series.
- The extension contains several large market events, which can affect the results.
- The two periods are not identical in many respects.
- The rolling windows overlap.

## Running the project

```bash
pip install -r requirements.txt
jupyter notebook
```

Run `01_replication.ipynb` first and then `02_extension.ipynb`.
