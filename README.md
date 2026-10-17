# Who is the GOAT?
### Federer. Nadal. Djokovic. Three extraordinary careers. One difficult question.

> **What happens when the greatest debate in tennis is examined point by point, rather than decided by a single trophy count?**

Roger Federer, Rafael Nadal, and Novak Djokovic redefined men's tennis in different ways. *Deconstructing Greatness* is a reproducible, explanatory sports-analytics study of their charted performances: serving, returning, winning, performing under pressure, adapting to surfaces, and facing one another.

The goal is **not** to manufacture an official GOAT verdict. It is to make the evidence—and the choices involved in interpreting it—visible to tennis professionals, analysts, and curious fans.

**Project format:** Executable Jupyter research report (`goat.ipynb`) · Auditable CSV exports · Statistical comparisons · Sensitivity and uncertainty analysis  
**Data:** [Jeff Sackmann's Tennis Match Charting Project](https://github.com/JeffSackmann/tennis_MatchChartingProject)  
**Study scope:** Charted matches involving Federer, Nadal, or Djokovic; **not** complete ATP career records.

---

## At a glance

| Player | Charted matches | Matches with known outcome | Verified wins | Win rate on known outcomes | Charted period |
|---|---:|---:|---:|---:|---|
| Roger Federer | 723 | 709 | 559 | **78.84%** | 1998–2021 |
| Rafael Nadal | 425 | 412 | 298 | **72.33%** | 2003–2024 |
| Novak Djokovic | 553 | 526 | 404 | **76.81%** | 2005–2026 |

**Read this table carefully:** these are *sample statistics from charted matches*, not official career win percentages. Charting coverage is selective, unequal across players, and extends through different years. Unknown match outcomes are excluded from the win-rate denominator, not counted as losses.

### A finding worth debating

Under the project's **balanced preference scenario**, which weights four analytical dimensions equally, Federer scores **70.65** and Djokovic **70.41**. Their difference is approximately **0.25 points**. A paired 95% uncertainty interval for that difference spans approximately **−2.97 to +3.70**: this analysis does **not** establish a statistically decisive separation between them.

The result is more interesting than a winner's podium: **reasonable analytical choices can change the ordering, and the evidence does not justify certainty where none exists.**

---

## Research questions

1. **Performance:** Who won most often *within the charted sample*, and what do point-level serve and return statistics reveal beyond match outcomes?
2. **Dominance:** Which player most consistently controlled service and return exchanges?
3. **Pressure:** How do measurable break-point, tiebreak, and deciding-set outcomes inform the clutch-performance discussion?
4. **Adaptability:** How does performance vary across hard, clay, and grass courts?
5. **Rivalries:** What changes when the analysis is restricted to Federer–Nadal, Federer–Djokovic, and Nadal–Djokovic meetings?
6. **The GOAT question:** How sensitive is any composite assessment to metric definitions, scaling, weighting, and uncertainty?

## KPI dictionary: how the tennis is measured

All rates use eligible observations for the stated player, period, surface, or matchup. Where numerator and denominator counts are available, the project aggregates the **counts first** and calculates the rate second; it does not blindly average match percentages.

| KPI | Formula | Interpretation |
|---|---|---|
| **Match win rate** | `100 × verified wins / matches with known outcomes` | Share of charted matches won where the outcome can be established |
| **Point win rate** | `100 × points won / eligible points played` | Share of observed points won |
| **Service-point win rate** | `100 × service points won / service points played` | Success when serving |
| **Return-point win rate** | `100 × return points won / return points played` | Success when receiving |
| **First-serve-in rate** | `100 × first serves in / eligible first-serve attempts` | First-serve consistency |
| **First-serve points won** | `100 × first-serve points won / first-serve points played` | Effectiveness behind the first serve |
| **Second-serve points won** | `100 × second-serve points won / second-serve points played` | Effectiveness behind the second serve |
| **Ace rate** | `100 × aces / eligible service points` | Aces relative to service-point exposure |
| **Break-point save rate** | `100 × break points saved / break points faced` | Success defending break points |
| **Tiebreak win rate** | `100 × tiebreaks won / tiebreaks played` | Success in completed tiebreaks |
| **Deciding-set win rate** | `100 × deciding sets won / deciding sets played` | Success in matches reaching a deciding set |
| **Surface win rate** | `100 × verified wins on surface / known outcomes on surface` | Sample win rate on hard, clay, or grass |
| **Head-to-head win rate** | `100 × verified wins versus rival / known outcomes versus rival` | Sample success against one of the other Big Three |

**Denominator discipline matters.** Missing, unavailable, or undefined observations should not silently become zeros. The notebook's `metric_contract` export is the authoritative reference for implemented eligibility rules, exclusions, and calculation details.

### Dominance Ratio (DR)

A serve-only statistic cannot tell the whole story. Dominance Ratio combines return effectiveness with the proportion of service points the player loses:

\[
DR = \frac{\text{Return points won}/\text{Return points played}}{1 - (\text{Service points won}/\text{Service points played})}
\]

A value above **1.0** indicates the player wins return points at a higher rate than they lose service points in the measured sample. It is a descriptive measure of point-level dominance, **not** a probability or a causal explanation of winning. Denominator edge cases require explicit handling.

Among reported overall results, Federer records approximately **1.277** and Djokovic **1.221**. These values describe the charted sample, not every match in either career.

### Clutch performance

The study examines three pressure-related components:

\[
BP_{save}=\frac{\text{Break points saved}}{\text{Break points faced}},\quad
TB_{win}=\frac{\text{Tiebreaks won}}{\text{Tiebreaks played}},\quad
DS_{win}=\frac{\text{Deciding sets won}}{\text{Deciding sets played}}
\]

These dimensions should be interpreted with their **sample sizes**: a strong percentage from a handful of opportunities does not carry the same evidential weight as one from hundreds. Consult `clutch_metrics` and `metric_contract` for the final implemented score definition, component eligibility, and missing-data rules. A clutch composite is an analytical convention, not an official tennis statistic.

### Surface adaptability

Performance is compared across **hard, clay, and grass**, separating two questions that are often conflated:

- **How strong was a player on each surface?** Look at surface-specific performance levels.
- **How consistent was performance across surfaces?** Look at variation across those levels.

Consistency alone does not mean superiority: a player can be similarly average on all surfaces, while another can be exceptional on two and less successful on a third. The `surface_adaptability` export and notebook document the implemented comparison and its eligibility rules.

### Preference scores: transparent judgments, not discovered truths

The project combines four dimensions into a **balanced preference scenario**:

\[
S_{balanced}=0.25P+0.25D+0.25C+0.25A
\]

where **P** = observed winning performance, **D** = dominance, **C** = clutch performance, and **A** = surface adaptability, each placed on the notebook's fixed-reference scoring scale before weighting.

The fixed reference and exact transformations matter: **do not substitute raw percentages or ratios directly into this equation**. See `preference_scores` and `metric_contract` for the implementation.

Alternative preference scenarios change the emphasis assigned to each dimension. They answer *“Who ranks higher under these stated priorities?”*—not *“Who is objectively the greatest?”*

---

## How the analysis works

```text
Jeff Sackmann Match Charting Project
                │
                ▼
     Source ingestion & inspection
                │
                ▼
    Player identity / outcome checks
                │
                ▼
      Big Three charted cohort
                │
                ▼
    Eligible observations and counts
                │
                ▼
     KPI & rivalry calculations
                │
                ▼
 Surface / pressure / dominance views
                │
                ▼
 Fixed-reference preference scenarios
                │
                ▼
     Uncertainty & sensitivity checks
                │
                ▼
  Jupyter report + validated CSV exports
```

### 1. Source selection and provenance

The underlying source is the community-driven **Tennis Match Charting Project**, which provides detailed charted match observations. The project filters its analytical universe to matches involving at least one of the Big Three, retaining relevant opponents for context. It does **not** assume charted matches constitute a random or complete record of their careers.

### 2. Cleaning and validation

The analysis reconciles player identities, surface labels, available match outcomes, and relevant denominators before publishing comparisons. A notable identity check concerns Nadal's playing hand: the source contained **420 left-handed and 3 right-handed indications**, and the project adjudicated his canonical playing hand as **left-handed** rather than accepting the minority inconsistency at face value.

### 3. Descriptive analytics

The notebook generates views of performance, clutch play, surface adaptability, head-to-head matchups, and scenario-based scores. The purpose is explanation and comparison—not forecasting the outcome of a future match.

### 4. Uncertainty and sensitivity

Small score differences should not be presented as settled rankings. The study examines uncertainty around comparisons and how results change when the definition of *greatness* changes. The balanced Federer–Djokovic comparison is a useful example: a narrow point estimate accompanies an interval crossing zero.

### 5. Export and quality control

The October 17 completed notebook run reported:

| Validation / deliverable | Reported result |
|---|---:|
| Notebook code cells | 37 |
| Total notebook cells | 93 |
| Named analytical views | 52 |
| Registered datasets | 37 |
| CSV exports | 39 |
| Figures | 19 |
| Numerical checks passed | 492 |
| Release checks passed | 42 |

The reported execution completed cleanly without warnings or errors. These are **run-specific validation results**, not a guarantee that every future environment or source revision will behave identically.

---

## Outputs and how to use them

**`goat.ipynb` is the technical source of truth.** The structured exports are intended to make its results easier to inspect, audit, and visualize without reconstructing every intermediate calculation.

| Analytical export | Purpose |
|---|---|
| `performance_metrics` | Core observed performance and efficiency measures |
| `clutch_metrics` | Pressure-related metrics and their component evidence |
| `surface_adaptability` | Hard/clay/grass comparisons and adaptability analysis |
| `head_to_head` | Direct rivalry analysis between the three players |
| `preference_scores` | Balanced and alternative weighted preference scenarios |
| `metric_contract` | KPI definitions, assumptions, and calculation/eligibility contracts |

**For tennis readers:** start with the *At a glance* table, then explore the surface and rivalry views before interpreting any composite score.

**For technical reviewers:** start with `goat.ipynb`, trace each reported metric to its numerator and denominator, inspect `metric_contract`, then review uncertainty, sensitivity, and release checks.

## Reproducing the research

1. Clone this repository and open it in VS Code or JupyterLab.
2. Create a Python environment and install the dependencies specified by the repository's environment or requirements file.
3. Obtain the source data from the [Tennis Match Charting Project](https://github.com/JeffSackmann/tennis_MatchChartingProject), observing its licensing and attribution terms.
4. Place the source files at the paths expected by the notebook and verify its input configuration.
5. Open **`goat.ipynb`**, restart the kernel, and run all cells in order.
6. Review generated figures, CSV exports, numerical checks, and release checks before interpreting the results.

> **Reproducibility note:** Exact source filenames, directory layout, package versions, and export paths should be checked against the final repository before publication. This README deliberately does not invent a one-command CLI or directory tree that may not exist in the finished implementation.

## Interpretation boundaries

This project takes the limitations of sports data seriously:

- **Charting selection bias:** The dataset is not a random sample of ATP matches; particular tournaments, opponents, periods, and memorable matches may be overrepresented.
- **Unequal coverage:** Federer, Nadal, and Djokovic have different numbers of charted matches and different covered career periods.
- **Known-outcome denominators:** A missing result is not a loss. Reported win rates exclude unknown outcomes.
- **Context matters:** Surface, era, opposition, tournament format, and match conditions may influence observed statistics.
- **Pressure metrics are descriptive:** They do not prove a stable psychological trait or explain why a player succeeded under pressure.
- **Composite scores depend on values:** Choosing dimensions, scaling methods, and weights is an explicit judgment, not a law of tennis.
- **Uncertainty is not optional:** A narrow numerical lead is not necessarily a meaningful difference.
- **No predictive claim:** The delivered study does not train or validate an ATP match-outcome or service-hold prediction model.

These constraints do not undermine the analysis. They define the boundary between what the evidence can support and what would be speculation.

## Data credit and attribution

Match-level and point-level source material is derived from **Jeff Sackmann's [Tennis Match Charting Project](https://github.com/JeffSackmann/tennis_MatchChartingProject)** and the work of its contributors. This repository is an independent analytical project and is not affiliated with or endorsed by the ATP, the players, or the source maintainers.

Respect the source repository's applicable licensing, attribution, and redistribution requirements when publishing raw data or derived artifacts. The analysis, interpretations, and preference scenarios are the responsibility of this project's author.

## Author

**Vishal Srikanth**  
*Who is the GOAT? — A Tennis Analytics Research Project*

---

## Final word: greatness is bigger than one number

Federer made tennis look effortless. Nadal turned persistence into an art form. Djokovic made impossible returns—and impossible comebacks—feel inevitable.

Statistics can reveal patterns behind those impressions. They can quantify who won more often in the observed matches, who performed better on serve and return, who handled measurable pressure situations, and how conclusions shift across surfaces and rivalries. They can also reveal something less comfortable: **sometimes the evidence is too close, too selective, or too dependent on our chosen definition to crown a single winner.**

That is the point of *Who is the GOAT?*. Not to end tennis's greatest argument, but to make it a better one.

**Three legends. Many dimensions of excellence. One debate worth examining carefully.**
