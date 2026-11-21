# DAZN Open Days: Measuring a Promotion with CausalImpact

Did DAZN's free Open Days in Spain (1 to 5 October 2026) actually raise interest in the platform, or would that bump have happened anyway? There was no A/B test to answer it, so I used public Google Trends data and CausalImpact (Bayesian structural time series) to estimate what would have happened without the promotion and compare it with what really happened.

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?logo=pandas&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?logo=tensorflow&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=white)

[`dazn_open_days.ipynb`](dazn_open_days.ipynb)

## Approach

- **Treated series:** "DAZN" searches in Spain.
- **Main control:** "LaLiga" searches in Spain. They follow the Spanish football calendar, but LaLiga matches were not part of the promotion.
- **Periods:** the model learns from 1 August to 30 September 2026 and evaluates 1 to 5 October 2026.
- **Checks:** a shorter training window, extra controls ("DAZN" searches in Germany and Italy) and a placebo test on a week with no promotion.

## Results

| Analysis | Relative effect | p-value | Prob. of causal effect |
|---|---|---|---|
| Main (LaLiga, from 1 Aug) | **+65.5%** | 0.033 | 96.7% |
| Robustness: training from 15 Aug | +68.8% | 0.058 | 94.2% |
| Robustness: + Germany and Italy | +115.6% | 0.006 | 99.4% |
| Placebo (24 to 28 Sep, no promotion) | +53.5% | 0.096 | 90.4% |

- The effect is positive in every analysis, so public data points to the Open Days having raised interest in DAZN in Spain.
- Its size is uncertain: it ranges from +65% to +116% depending on the setup, and the placebo already finds about +50% in a week with no promotion. Both weeks fall inside an international break, when LaLiga searches collapse, so the real effect is probably smaller than these numbers.
- Without a parallel test, the result depends on the analyst's choices, which is why the checks are reported next to the main number.

## Reusing it with internal data

The same steps work with internal data: replace the treated series with daily sign-ups, use markets or products that were not exposed to the campaign as controls, update the pre and post periods, and keep the robustness and placebo checks.

## Data

Daily Google Trends interest from 1 August to 15 October 2026, in [`Data/`](Data/). Google Trends is a 0-100 index relative to the busiest day of each series, not a count of searches or subscriptions, so this measures public interest only.

## Run it

```bash
git clone https://github.com/said-velasquez/dazn-open-days-causal-impact.git
cd dazn-open-days-causal-impact
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook
```

`tfcausalimpact` installs TensorFlow (around 500 MB). I ran the notebook with Python 3.11.

## Tech stack

Python, pandas, Matplotlib, tfcausalimpact (TensorFlow Probability), Jupyter.

## Author

**Said Velásquez**, Computer Engineering student based in Madrid.
[LinkedIn](https://www.linkedin.com/in/said-velasquez) · [GitHub](https://github.com/said-velasquez)

## License

MIT, see [`LICENSE`](LICENSE).
