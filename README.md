# NGO Press Campaigns and Bank Stock Reactions (Event Study, CAPM, CAR)

This repository contains a finance with big data class project that studies whether NGO press campaigns are associated with abnormal stock return dynamics for banks in the US and Europe.

The notebook walks through:
* importing and cleaning NGO campaign data (Sigwatch style dataset)
* exploratory analysis of key campaign variables (sentiment, NGO power, prominence)
* building a bank returns panel (prices, returns, market excess return, risk free rate)
* estimating rolling CAPM parameters (alpha and beta)
* computing Cumulative Abnormal Returns (CAR) around campaign dates using an event study approach

## Project motivation

Responsible investment often depends on information intermediaries such as ESG news, media coverage, and NGO campaigns. This notebook explores a concrete mechanism: do press campaigns coincide with measurable market reactions in bank equities?

## Data (not included)

The original coursework used:
* NGO press campaign data (campaign date, targeted company, sentiment, prominence, NGO power, etc.)
* bank price and return data (Datastream style export)
* market excess returns and risk free rates (Fama French style factors)

Because these datasets were provided for coursework, they may not be redistributable. The notebook expects a local `data/` folder with the relevant files.

## Methodology overview

### 1) Campaign data exploration (Day 1)
* Load and append multiple years of NGO campaign files into a single dataset
* Filter to bank related campaigns and summarize:
  * number of campaigns, NGOs, and targeted firms
  * distribution and patterns in sentiment and prominence
  * NGO power as a proxy for influence or reach

### 2) Optional extensions (Day 1)
The notebook includes two exploratory extensions:
* Social media attention via Reddit (attempted). Implementation is present, but historical scraping constraints limited results.
* Link based sentiment analysis (attempted). The notebook fetches text (when available) from campaign links and applies TextBlob sentiment, then compares it conceptually to dataset sentiment.

### 3) Returns panel construction (Day 2)
* Import bank price levels and compute simple returns
* Merge with market excess returns and risk free rates
* Maintain consistent date alignment across sources

### 4) Rolling CAPM estimation (Task 4)
For each bank, estimate CAPM parameters over a rolling window:
* choose market series based on geography (US banks vs EU banks)
* run OLS: bank excess return on market excess return
* store time series of alpha and beta estimates (plus p values)

### 5) Event study and CAR computation (Task 5)
For each campaign event:
* define an estimation window ending 50 trading days before the event
* fit CAPM on the estimation window to obtain alpha and beta
* compute abnormal returns around the event in a ±10 trading day window
* sum abnormal returns to obtain CAR for that event

The notebook then aggregates CAR outcomes across events to provide bank level comparisons and quick visual summaries.

## Outputs

The notebook produces:
* descriptive statistics and plots for campaign variables
* time series plots for rolling beta and alpha (overall and split by region)
* CAR estimates per event, with aggregated views by bank and by campaign sentiment sign

## Notes on reproducibility

Some steps depend on external links and platform policies (for example, old Reddit content or dead campaign URLs). If those sources change or disappear, the notebook still runs for the core CAPM and CAR pipeline as long as the structured datasets are available.

## Potential improvements

* Replace link scraping sentiment with a more robust pipeline (language detection, deduplication, transformer based sentiment).
* Add significance testing and subgroup analysis (for example, pre vs post COP21, campaign prominence buckets, NGO power buckets).
* Rework entity matching between campaign company names and bank tickers using fuzzy matching with manual review.
* Package the notebook logic into reusable modules and add a small CLI.

## Authors

Group 2: Emanuele Sala, Luca Soleri, Fabio Stefana

## Acknowledgements

Course project for “Finance with Big Data”, hackathon topic: NGO press campaigns and responsible investment.


