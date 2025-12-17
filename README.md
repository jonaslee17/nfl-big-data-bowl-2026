# Pre-Snap Coverage Decisions: Quantifying Cornerback Strategy Before the Route

**NFL Big Data Bowl 2026 Submission**

## Overview

This project analyzes cornerback positioning strategy in the critical seconds before the quarterback releases the ball in man coverage situations. Using player tracking data from the 2023 NFL season, we quantify pre-throw decision-making through a novel "growth rate" metric and demonstrate its statistically significant impact on Expected Points Added (EPA).

## Key Findings

- Pre-throw cornerback strategy significantly affects offensive EPA (ANOVA: F=3.074, p=0.0156)
- "Maintained" coverage (matching receiver movement) produces optimal results (+1.593 EPA across 736 plays)
- Average cornerback behavior shows pressing forward (-0.108 yards/frame) rather than backpedaling
- Strategy effectiveness varies by route type, with cornerbacks adjusting aggression based on anticipated concepts

## Dataset

**Source**: NFL Big Data Bowl 2026 tracking data (2023 season)

**Scope**: 
- 1,439 cornerback-receiver pairings
- 271 games across 18 weeks
- Man coverage completions only
- Weeks 1-18 of 2023 regular season

**Data Files**:
- `input_2023_w*.csv`: Pre-throw tracking data (snap to ball release)
- `output_2023_w*.csv`: Post-throw tracking data (release to catch)
- `supplementary_data.csv`: Play-level metadata including EPA, route type, down/distance

## Methodology

### Phase 1: Data Preparation and Player Pairing

**Algorithm**: Weighted distance pairing prioritizing lateral alignment
- Y-axis (lateral) weight: 2.0
- X-axis (depth) weight: 1.0
- Rationale: Cornerbacks align horizontally with receivers in man coverage regardless of depth

**Output**: `cushion_analysis_data.csv` containing initial cornerback-receiver pairings with baseline cushion measurements

### Phase 2: Post-Throw Analysis

**Metric**: Erosion rate (yards per frame of cushion change after ball release)

**Key Result**: 66% of cornerbacks maintain cushion within ±0.05 yards/frame, with average erosion of -0.009 yards/frame

**Output**: `erosion_analysis_data.csv` containing post-throw execution metrics

### Phase 3: Pre-Throw Analysis

**Primary Metric**: Growth rate (yards per frame of cushion change from snap to throw)

Formula: `Growth Rate = (Cushion at Throw - Cushion at Snap) / Number of Frames`

**Pattern Classification**:
- Aggressive Backpedal: > +0.3 y/frame
- Moderate Backpedal: +0.1 to +0.3 y/frame
- Maintained: -0.1 to +0.1 y/frame
- Moderate Press: -0.3 to -0.1 y/frame
- Aggressive Press: < -0.3 y/frame

**Output**: `prethrow_coverage_data.csv` containing pre-throw strategy metrics

### Phase 4: Combined Statistical Analysis

**Tests Performed**:
- One-way ANOVA: Pre-throw pattern vs EPA
- Pearson correlation: Growth rate vs EPA
- Route-specific strategy analysis

**Output**: `final_combined_analysis.csv` containing all metrics merged with statistical results

## Repository Structure
```
nfl-big-data-bowl-2026-analytics/
├── data/
│   ├── train/
│   │   ├── input_2023_w*.csv          # Pre-throw tracking data
│   │   └── output_2023_w*.csv         # Post-throw tracking data
│   ├── supplementary_data.csv         # Play metadata
│   └── processed/
│       ├── cushion_analysis_data.csv  # Notebook 01 output
│       ├── erosion_analysis_data.csv  # Notebook 02 output
│       ├── prethrow_coverage_data.csv # Notebook 03 output
│       └── final_combined_analysis.csv # Notebook 04 output
├── notebooks/
│   ├── 01_data_prep.ipynb             # CB-WR pairing
│   ├── 02_erosion_calculation.ipynb   # Post-throw analysis
│   ├── 03_prethrow_coverage.ipynb     # Pre-throw analysis
│   └── 04_combined_analysis.ipynb     # Statistical testing
├── visualizations/
│   ├── test_pairing.png               # Example pairing
│   ├── erosion_conclusion.png         # Post-throw patterns
│   ├── test_prethrow_coverage.png     # Pre-throw timeline
│   └── combined_analysis_summary.png  # 4-panel results
├── requirements.txt
└── README.md
```

## Installation and Setup

### Requirements
```
pandas>=1.5.0
numpy>=1.23.0
matplotlib>=3.6.0
seaborn>=0.12.0
scipy>=1.9.0
jupyter>=1.0.0
```

### Installation
```bash
# Clone repository
git clone <repository-url>
cd nfl-big-data-bowl-2026-analytics

# Install dependencies
pip install -r requirements.txt

# Launch Jupyter
jupyter notebook
```

## Usage

### Running the Analysis Pipeline

Execute notebooks in sequence:

**1. Data Preparation (01_data_prep.ipynb)**
- Runtime: ~5 minutes
- Pairs cornerbacks with receivers
- Creates baseline cushion measurements
- Output: `cushion_analysis_data.csv`

**2. Post-Throw Analysis (02_erosion_calculation.ipynb)**
- Runtime: ~3 minutes
- Analyzes cushion changes after ball release
- Calculates erosion rates and patterns
- Output: `erosion_analysis_data.csv`

**3. Pre-Throw Analysis (03_prethrow_coverage.ipynb)**
- Runtime: ~8 minutes
- Processes all INPUT files (snap to throw phase)
- Calculates growth rate metric
- Classifies coverage strategies
- Output: `prethrow_coverage_data.csv`

**4. Combined Analysis (04_combined_analysis.ipynb)**
- Runtime: ~2 minutes
- Merges all datasets
- Performs statistical tests
- Generates visualizations
- Output: `final_combined_analysis.csv` and summary visualization

**Total Pipeline Runtime**: Approximately 18 minutes

### Key Metrics

**Growth Rate**: Primary metric for pre-throw strategy
- Negative values indicate pressing (closing cushion)
- Positive values indicate backpedaling (giving cushion)
- Normalized per frame to account for varying play durations

**Erosion Rate**: Secondary metric for post-throw execution
- Measures cushion maintenance after ball release
- Generally shows minimal variance across plays

## Results Summary

### Statistical Significance

One-way ANOVA demonstrates pre-throw strategy significantly affects EPA:
- F-statistic: 3.074
- p-value: 0.0156
- Conclusion: Pre-snap positioning decisions measurably impact defensive success

### Strategy Performance by Pattern

| Pattern | Mean EPA | Sample Size | Usage Rate |
|---------|----------|-------------|------------|
| Moderate Backpedal | +1.714 | 27 | 2% |
| Maintained | +1.593 | 736 | 51% |
| Moderate Press | +1.330 | 606 | 42% |
| Aggressive Press | +1.319 | 67 | 5% |
| Aggressive Backpedal | +0.904 | 3 | <1% |

### Route-Specific Strategy

| Route | Avg Growth Rate | Avg EPA | Interpretation |
|-------|-----------------|---------|----------------|
| GO | -0.073 y/frame | +2.745 | Minimal pressing on deep shots |
| CROSS | -0.135 y/frame | +1.432 | Most aggressive pressing |
| HITCH | -0.117 y/frame | +0.891 | Moderate press, lowest EPA |
| OUT | -0.112 y/frame | +1.249 | Standard pressing approach |
| SLANT | -0.090 y/frame | +0.997 | Less aggressive on quick routes |

## Practical Applications

### For Defensive Coordinators

1. Emphasize maintained coverage over aggressive pressing as base strategy
2. Reserve aggressive press for specific situational advantages
3. Incorporate route recognition training to optimize pre-snap adjustments

### For Cornerback Development

1. Coach discipline and position-matching over pure aggression
2. Use growth rate as quantifiable feedback metric in practice
3. Teach situational awareness for strategy selection based on down/distance and formation

### For Offensive Coordinators

1. Exploit aggressive press tendencies with double moves and vertical routes
2. Design precise route timing to attack maintained coverage
3. Use formation adjustments to create pre-snap positioning dilemmas

## Limitations

- Sample size constraints for rare strategies (aggressive backpedal: n=3)
- Confounding variables inherent to football analysis (QB skill, protection, scheme)
- EPA measures play outcome rather than pure coverage quality
- Analysis aggregates across receiver skill levels
- Single season data limits generalizability

## Future Research Directions

1. Individual cornerback profiling across multiple seasons
2. Down-distance contextual analysis
3. Formation-specific strategy breakdowns (slot vs outside, bunch sets)
4. Quarterback decision-making response to cornerback positioning
5. Multi-season trend analysis of strategy evolution

## Technical Notes

### Data Processing

- Frame-by-frame distance calculations using Euclidean distance
- Per-frame normalization accounts for varying quarterback release times
- Weighted pairing algorithm minimizes incorrect cornerback-receiver assignments

### Statistical Methods

- One-way ANOVA for categorical strategy comparison
- Pearson correlation for continuous metric relationships
- Threshold-based pattern classification derived from data distribution

### Validation

- Manual inspection of high-leverage pairings
- Cross-validation of pairing algorithm accuracy
- Sensitivity analysis on pattern classification thresholds

## Citation

If you use this analysis or methodology in your work, please cite:
```
Pre-Snap Coverage Decisions: Quantifying Cornerback Strategy Before the Route
NFL Big Data Bowl 2026
Dataset: 2023 NFL Season Tracking Data
```

## License

This project uses data provided by the NFL Big Data Bowl 2026 competition. All analysis code is provided for educational and research purposes.

## Contact

For questions about methodology or findings, please open an issue in this repository.

## Acknowledgments

- NFL Big Data Bowl 2026 for providing tracking data
- NFL Next Gen Stats for player tracking technology
- Kaggle for hosting the competition platform