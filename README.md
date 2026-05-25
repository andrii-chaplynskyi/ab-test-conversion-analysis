# A/B Test Conversion Analysis

End-to-end A/B test analysis of an e-commerce funnel. The project covers experiment framing, conversion metric calculation, statistical significance testing with a two-proportion z-test, and a business recommendation based on the results.

## Business Context

An online retailer introduced a UI or flow change to the purchase funnel and ran a controlled experiment to measure its effect. The analysis treats the dataset as a standard product analytics task: define the metrics, compare control and test groups, apply a rigorous statistical test, and deliver a clear recommendation rather than just a list of p-values.

## Business Questions

- Did the test group improve conversion compared with the control group?
- Which funnel steps changed the most?
- Are the observed differences statistically significant, or likely caused by random variation?
- Should the business implement the tested change?

## Dataset

- Source: BigQuery training dataset (`data-analytics-mate.DA`).
- Structure: session-level event data with group assignment (`control` / `test`) and funnel event names.
- Preparation: event names were standardised; sessions were pivoted into a flat metric table with one row per session showing which funnel steps were reached.

## Methodology

1. **Data preparation** — load event log, standardise event names, assign sessions to control or test group.
2. **Conversion metric calculation** — compute step-level conversion rates (users who reached step N divided by users who entered the funnel).
3. **Group comparison** — calculate absolute difference and relative uplift between test and control.
4. **Statistical significance** — two-proportion z-test (`statsmodels.stats.proportion.proportions_ztest`); threshold set at α = 0.05.
5. **Business recommendation** — interpret results in terms of whether the observed uplift is reliable enough to justify a full rollout.

The SQL query used to extract and pre-aggregate the raw event data is included as a separate file (`sql_ab_testing_tool.md`).

## Key Findings

- Conversion differences were calculated for each funnel step between control and test groups.
- Statistical significance was evaluated using a two-proportion z-test; results are reported with exact z-statistics and p-values.
- The notebook retains non-significant results rather than filtering them, because a negative result is informative for the business decision.

Full numerical outputs are in the notebook. Figures in this README are not inferred — all values come directly from notebook outputs.

## Tools and Technologies

- **SQL** (BigQuery dialect) for data extraction and pre-aggregation
- **Python**: `pandas`, `numpy`, `statsmodels`
- **Statistical method**: two-proportion z-test for conversion rate comparison
- **Environment**: Google Colab

## Repository Structure

```
.
├── README.md
├── notebooks/
│   └── ab_test_conversion_analysis.ipynb
└── sql_ab_testing_tool.md
```

## How to Run

1. Clone the repository and open the notebook in Google Colab.
2. Install dependencies: `pip install pandas numpy statsmodels`.
3. Run the notebook top to bottom. All outputs are committed so the notebook can be read without re-execution.

## Portfolio Value

This project demonstrates practical A/B testing methodology in a product analytics context: experiment framing, metric design, z-test application, and cautious interpretation of statistical results — including knowing when a result does not support a rollout decision.
