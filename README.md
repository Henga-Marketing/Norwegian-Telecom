# Norwegian-Telecom
# Norway Telecom Analysis: Telia Mobile Performance

A Python portfolio project exploring mobile subscriptions, service usage and reported subscription revenue in Norway, with a focus on Telia. The aim is to turn telecom market data into clear business questions, charts and decision support.

This is an independent learning project and is not affiliated with Telia.

## Business questions

- How did Telia's postpaid subscriptions change between 2023 and 2025?
- How do consumer and business segments differ?
- How did the consumer mix of postpaid and prepaid subscriptions change?
- How did data traffic and SMS usage develop?
- Did reported subscription revenue grow faster than postpaid subscriptions in 2023–2024?
- How much of the revenue change is associated with subscription counts and approximate revenue per subscription?

## Dataset

The analysis uses `Ekomstat_Alle-data(1).csv`, a Norwegian telecom statistics export containing operator-level aggregate observations. The supplied file has 62,903 rows and 12 columns, with annual and half-year reports from 2015 to 2025.

The main analysis selects:

- Operator: `telia norge`
- Main category: `Mobiltjenester`
- Service: `Mobiltelefoni`
- Segments: `Privat` (consumer) and `Bedrift` (business)
- Period: annual reports (`Helår`)

| Field | Description |
| --- | --- |
| `Tilbyder` | Operator |
| `Hovedkategori`, `Delkategori` | Service categories |
| `Teknologi` | Technology |
| `Hovedgruppe` | Metric group, such as subscriptions, revenue or traffic |
| `Nivå1`–`Nivå4` | Metric detail |
| `Markedssegment` | Market segment |
| `Rapport` | Reporting period |
| `svar` | Reported metric value |

Data provenance note: the export filename is retained here. The original publisher URL and metric documentation should accompany any public dataset release. Source data is not licensed by this README.

## Analysis workflow

1. Preserve the original dataset and inspect its structure.
2. Check missing values, numeric validity, repeated rows and repeated descriptive keys.
3. Extract year and report type from `Rapport`.
4. Select explicit metric definitions before calculating totals or ratios.
5. Compare subscription counts, annual growth and consumer subscription mix.
6. Plot data and SMS traffic separately because they have different units.
7. Index comparable series to a 2023 baseline of 100.
8. Calculate approximate reported revenue per subscription.
9. Decompose the 2023–2024 revenue change and check that the effects reconcile to the observed change.

## Visualisations

- Subscription trends by segment
- Annual subscription growth
- Consumer postpaid/prepaid mix
- Data traffic and SMS trends
- Usage indices with 2023 = 100
- Revenue and subscription growth comparison
- Approximate revenue per subscription
- Revenue change waterfall by segment

Charts describe aggregate patterns. They do not establish the causes of changes or measure campaign effectiveness.

## Important reporting change

The supplied Telia mobile data uses `Fra abonnement-gammel` for subscription revenue in 2023 and 2024, and `Fra abonnement-ny` in 2025.

These definitions are not automatically combined. Revenue comparisons and the waterfall use **2023–2024**. Subscription and usage analyses can cover **2023–2025**, subject to metric consistency checks.

## Revenue decomposition

Define reported revenue as `R = N × V`, where `N` is the reported postpaid subscription count and `V = R / N` is approximate revenue per subscription.

The symmetric decomposition is:

```text
Subscription count effect = (N1 - N0) × (V0 + V1) / 2
Revenue-per-subscription effect = (V1 - V0) × (N0 + N1) / 2
```

The two effects sum to the revenue change. This is an accounting decomposition, not a causal estimate. Matching the scope of revenue and subscriptions still requires source documentation.

## Tools and running the notebook

Python, Jupyter Notebook, pandas, NumPy, Matplotlib and Seaborn.

Install the libraries in a notebook cell:

```python
%pip install pandas numpy matplotlib seaborn
```

Place the CSV in a local `data` folder and update the notebook's loading cell:

```python
from pathlib import Path
import pandas as pd

file_path = Path("data") / "Ekomstat_Alle-data(1).csv"
df_raw = pd.read_csv(file_path, sep=",", encoding="utf-8-sig")
```

Run cells from top to bottom, including after restarting the kernel. To display charts in Jupyter, use `%matplotlib inline` and remove any `matplotlib.use("Agg")` setting.

## Limitations

- Observations are aggregate metrics, not individual customers. Subscription counts are not unique customer counts.
- Revenue and traffic units must be confirmed from source documentation; unverified charts retain source-unit labels.
- Revenue divided by a reported end-of-period subscription count is an approximation, not a verified ARPU measure.
- Missing observations are not treated as zero.
- Repeated descriptive keys can have different values; they must be investigated before aggregation.
- Annual and half-year totals are not added together.
- The dataset cannot support customer-level churn prediction, CLV, personalisation, campaign attribution or a real A/B test.

## Next steps

- Validate source definitions and units.
- Review cross-operator data quality before building Norway market opportunity comparisons.
- Export validated tables and build a Power BI dashboard. The Power BI dashboard is planned and is not part of the current Python analysis.

## Author

Hengameh Mohammadinakhjiri

MSc Strategic Marketing Management, BI Norwegian Business School
