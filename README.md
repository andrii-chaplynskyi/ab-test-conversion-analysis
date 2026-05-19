# A/B Test Conversion Analysis

## Project Overview

This project analyses A/B test results for e-commerce funnel metrics. The goal is to compare control and test groups, calculate conversion rates, estimate relative changes and evaluate statistical significance.

## Business Questions

- Did the test group improve conversion compared with the control group?
- Which funnel metrics changed the most?
- Are observed differences statistically significant or likely caused by random variation?
- Should the business implement the tested changes?

## Tools and Technologies

- Python
- pandas
- numpy
- statsmodels
- z-test for proportions
- Tableau
- Jupyter / Google Colab

## Methods Used

- Data preparation and event name standardisation
- Session-level conversion metric calculation
- Control vs test group comparison
- Relative uplift calculation
- z-statistic and p-value calculation
- Statistical significance interpretation
- Tableau dashboard preparation

## Key Portfolio Value

This project is useful for product analytics roles because it demonstrates practical A/B testing, conversion analysis and cautious interpretation of p-values.

## Current Status

The project is a good second portfolio project, but it needs stronger written explanation before public release.

## Improvements Before Public Release

- Expand the README with dataset description and experiment context.
- Add a clear methodology section explaining the conversion formulas.
- Add a final business recommendation for each test.
- Explain limitations such as sample size and statistical power.
- Add Tableau dashboard screenshots and public dashboard link.
- Ensure all notebook text is in English.

## Files

- `notebooks/ab_test_conversion_analysis.ipynb` - main A/B testing notebook.
- `sql_ab_testing_tool.md` - supporting SQL query used to prepare A/B testing data.

