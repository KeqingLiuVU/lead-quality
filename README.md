# Lead Quality Analysis

A business-focused analytics case study that identifies the strongest drivers of lead quality and evaluates whether traffic filtering can economically achieve a 20% quality-lift incentive.

## Business question

Lead quality varies across acquisition sources, consumer profiles, and creative configurations. The analysis answers three practical questions:

1. Is lead quality improving or deteriorating over time?
2. Which attributes are independently associated with higher-quality leads?
3. Can the advertiser reach a 20% lift in Closed Rate without sacrificing too much volume?

## Executive result

The dataset's overall **Closed Rate is 8.1%**, closely matching the stated 8.0% baseline. The incentive target is therefore approximately **9.7%**.

The analysis finds that:

- **Acquisition source/channel** and **consumer debt level** are the clearest quality drivers.
- The **$50,001–$90,000 debt segment** has the strongest quality among debt groups.
- Creative differences are visible descriptively, but they weaken after controlling for overlapping campaign variables.
- Strict filters can exceed the target rate, but retain too little traffic to offset the CPL change from **$30 to $33**.
- The stronger business strategy is to test scaling high-quality segments while improving weaker traffic, rather than applying a hard filter.

## Analytical workflow

Raw lead data → outcome mapping → weekly trend analysis → segment comparison → Cramér's V → multivariate logistic regression → CPL scenario analysis → recommendations

## Methods

- Data cleaning and outcome categorization
- Closed, positive-quality, and bad-quality rate construction
- Weekly trend testing
- Segment performance and volume comparison
- Cramér's V analysis for overlapping categorical variables
- Multivariate logistic regression for controlled driver analysis
- Scenario modeling for quality, retained volume, and revenue trade-offs

## Key metrics

| Metric | Definition |
|---|---|
| Closed Rate | Closed leads / total leads |
| Positive Quality Rate | (Closed + other positive-quality leads) / total leads |
| Bad Lead Quality Rate | Bad-quality leads / total leads |
| Incentive Target | Approximately 9.7% Closed Rate |
| Break-even Volume | Approximately 91% of current traffic at the higher CPL |

## Repository contents

- [case_study.ipynb](./case_study.ipynb) — reproducible analysis and modeling workflow
- [REPORT.md](./REPORT.md) — detailed findings, interpretation, and recommendations

## How to review

For a quick business read, start with [REPORT.md](./REPORT.md). For the analytical implementation, open [case_study.ipynb](./case_study.ipynb) in Jupyter Notebook or JupyterLab and run the cells in order in a standard Python data-science environment.

## Decision recommendations

1. Test whether high-quality acquisition traffic can scale without degrading.
2. Prioritize the strongest debt segment in controlled campaigns.
3. Investigate and improve the weakest debt ranges before removing volume.
4. Use phone-quality signals as supporting features rather than hard filters.
5. Run controlled creative tests because creative and campaign variables overlap.
6. Add acquisition cost and margin by source before making a final profit decision.

## Limitations

- Segment results are observational and should not be interpreted as causal.
- Some high-performing segments have limited volume.
- Creative variables are confounded with campaign configuration.
- The available analysis evaluates revenue trade-offs; source-level cost is needed for a complete profit view.
- All portfolio work should use data that is public, synthetic, or otherwise approved for sharing.

## Author

Keqing Liu — Data Scientist focused on machine learning, forecasting, NLP, and production-quality analytics.
