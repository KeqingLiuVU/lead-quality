# Aarki Analytics Challenge

**Candidate:** Keqing Li  

## Executive Summary

Lead quality varies meaningfully across acquisition source and consumer debt level. The strongest signals in this dataset are:

- **Acquisition source/channel:** `PartnerModel` is one of the clearest drivers of lead quality.
- **Consumer profile:** `DebtLevelClean` is also a clear driver, with the `50001-90000` debt range showing the strongest quality.
- **Ad creative:** Widget design shows descriptive differences, but it is less convincing as an independent driver after controlling for other variables.

For the CPL incentive question, the dataset's overall Closed Rate is **8.1%**, which matches the prompt's 8.0% baseline. A 20% quality lift would require a Closed Rate of about **9.7%**. Some high-quality segments exceed this target, but they are too small to reach the incentive economically through simple filtering. The best opportunity is to test scaling higher-quality traffic sources and stronger debt segments, rather than hard-filtering current volume.

## Metric Definitions

I grouped `CallStatus` into four business categories:

- **Closed:** the lead became a customer. This is the advertiser's strongest success metric.
- **Good Lead Quality:** EP Sent, EP Received, or EP Confirmed.
- **Bad Lead Quality:** unable to contact, invalid profile, or does not qualify.
- **Unknown:** neutral outcomes that are neither clearly good nor bad.

The main metrics used are:

- **Closed Rate:** `Closed leads / total leads`
- **Positive Quality Rate:** `(Closed + Good Lead Quality leads) / total leads`
- **Bad Lead Quality Rate:** `Bad Lead Quality leads / total leads`

For the CPL opportunity analysis, I use **Closed Rate** as the primary metric because the prompt's 8.0% baseline matches the dataset's actual Closed Rate of 8.1%.

## 1. Lead Quality Trends Over Time

![Weekly Lead Quality Rates](images/weekly_lead_quality_rates.png)

Weekly lead quality is noisy, but the strict Closed Rate appears to weaken over time, while Bad Lead Quality increases. The broader Positive Quality Rate fluctuates and does not show a clear statistically significant trend.

Statistical trend tests show:

- **Closed Rate:** statistically significant decline over time.
- **Bad Lead Quality Rate:** statistically significant increase over time.
- **Positive Quality Rate:** no statistically significant trend.

Overall, the data does not show a clean improvement in lead quality over time. If anything, the strict success metric weakens while bad quality increases.

## 2. Drivers of Lead Quality

I first reviewed lead quality by segment, then checked whether segment variables were highly overlapping. This matters because several fields describe nearly the same business split.

![Lead Quality by Segment](images/driver_segment_rates.png)

The segment view shows that quality is not evenly distributed. `Adknowledge` has the strongest acquisition-source performance, while the `50001-90000` debt range is the strongest debt segment. However, these stronger segments do not represent the majority of volume, so they are opportunities to investigate rather than automatic answers.

![Segment Association Heatmap](images/cramers_v_heatmap.png)

Key association findings:

- `PublisherCampaignName`, `PublisherZoneName`, and `Partner` are highly overlapping, so I use `PartnerModel` as the representative acquisition/source variable.
- `AdvertiserCampaignName` and `WidgetDesign` are highly overlapping, so creative findings should be interpreted carefully.
- `WidgetColor` is also strongly tied to `WidgetDesign`, so it is not treated as a standalone driver.

### Segment Findings

The strongest descriptive segments are:

- **Adknowledge:** higher Closed Rate and Positive Quality Rate than Google and Yahoo, but low volume.
- **Call Center:** relatively strong Positive Quality Rate, but also higher Bad Quality Rate.
- **DebtLevelClean = 50001-90000:** strongest debt segment by both Closed Rate and Positive Quality Rate.

### Controlled Driver Check

To avoid over-interpreting overlapping variables, I ran a multivariate logistic regression using:

`IsPositiveQuality ~ PartnerModel + DebtLevelClean + WidgetDesign + PhoneScoreGroup`

The model suggests:

- `PartnerModel` remains statistically significant.
- `DebtLevelClean` remains statistically significant.
- `PhoneScoreGroup` shows directional signal but is not statistically significant at the 0.05 level.
- `WidgetDesign` is not statistically significant after controlling for the other variables.

Conclusion: acquisition source/channel and consumer debt level are the clearest drivers of lead quality in this dataset.

## 3. CPL Opportunity Analysis

The advertiser offers a higher CPL if lead quality improves by 20%. Since the dataset's Closed Rate is 8.1%, the target is:

- **Current Closed Rate:** 8.1%
- **Target Closed Rate:** 9.7%
- **Current closed leads:** 245
- **Required closed leads at current volume:** 294
- **Additional closed leads needed:** 49

### Scenario Results

![CPL Scenario Analysis](images/cpl_scenario_analysis.png)

Strict filters can hit the target Closed Rate, but they retain too little volume. For example:

- Keeping only `Adknowledge` reaches a Closed Rate above target, but retains only about 6% of volume.
- Keeping only `DebtLevelClean = 50001-90000` reaches the target, but retains only about 12% of volume.
- Broader filters still retain too little volume to offset the small CPL increase from $30 to $33.

Because CPL only increases from $30 to $33, revenue breaks even only if roughly 91% of current volume is retained. The target-hitting filtering scenarios retain far less than that.

### Remove-Worst Strategy

I also tested removing the weakest segments first instead of keeping only the strongest ones.

![Remove Worst Segment Tradeoff](images/remove_worst_tradeoff.png)

- Removing only `DebtLevelClean = 90000+` is a low-risk improvement because it keeps about 91% of volume and nearly breaks even on revenue.
- However, it only raises Closed Rate to about 8.36%, below the 9.7% target.
- To actually hit the target, filtering still removes too much volume.

### Positive Quality Sensitivity Check

Using the broader Positive Quality Rate leads to the same business conclusion. Some segments exceed the 20% lift target, but filtering out enough low-quality volume to hit the target still removes too much total volume.

## Recommendations

I would not recommend using a hard filter as the main strategy to reach the CPL incentive. The better path is mix optimization and testing:

1. **Test scaling Adknowledge traffic.** It has strong quality, but current volume is small. Validate whether quality holds as volume increases.
2. **Prioritize the `50001-90000` debt range.** This is the strongest consumer-profile segment.
3. **Investigate the weakest debt ranges, especially `90000+`.** Removing or improving this segment may be a low-risk quality improvement, even though it does not hit the full target.
4. **Use PhoneScore as a soft signal, not a hard filter.** Coverage is incomplete and the model evidence is directional rather than definitive.
5. **Run controlled creative tests.** Widget differences are visible descriptively, but creative variables are confounded with campaign setup.
6. **Collect acquisition cost by source.** This analysis evaluates revenue, not profit. Final decisions require cost and margin by channel.

## Final Takeaway

There are real lead quality signals in the data, especially by acquisition channel and debt level. However, the high-quality pockets are not large enough to reach the CPL incentive through filtering alone without losing too much volume. The practical opportunity is to test whether those high-quality pockets can be scaled while improving or reducing weaker sources.
