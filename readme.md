# Formula 1 Qualifying Impact Analysis

**Analyzing the Relationship Between Starting Position and Race Results**



## Project Overview

**Objective:** To quantify how Formula 1 qualifying positions influence final race results and to test the common belief that "qualifying is everything" in modern F1 racing.

**Key Questions:**

  - How strongly does qualifying position predict the final race finish?
  - Do certain circuits enable more position changes, and can we quantify this?
  - How have major regulation changes affected overtaking opportunities?
  - Which drivers consistently outperform their qualifying positions?

**Analysis Period:** 1950-2023 Seasons
**Records Analyzed:**

  - 26,759 race results
  - 10,494 qualifying sessions
  - 1,125 races across 77 circuits


## Methodology

### Data Pipeline

```mermaid
graph LR
    A[Kaggle Dataset] --> B[Data Extraction]
    B --> C[SQLite Database]
    C --> D[Data Cleaning & Prep]
    D --> E[Feature Engineering]
    E --> F[Statistical Analysis]
    F --> G[ML Modeling]
    G --> H[Visualization]
```

### Key Steps

1.  **Data Collection:** Acquired historical F1 data from Kaggle (15 CSV files) and structured it in an SQLite database for efficient querying.

2.  **Data Processing:**

      - Merged qualifying and race results using `raceId` and `driverId`.
      - Calculated `positions_gained` for all **classified finishers**. Non-finishers (DNFs) were excluded from this calculation to avoid skewing the averages.
        ```python
        df['positions_gained'] = df['qual_position'] - df['race_position']
        ```
      - Engineered a `regulation_era` feature to group races by major rule sets:
          - **V10 Era (1993-2005)**
          - **V8 Era (2006-2013)**
          - **Hybrid Era (2014-2021)**
          - **Ground Effect Era (2022-Present)**

3.  **Analysis Techniques:**

      - Correlation analysis to measure the strength of the relationship between starting and finishing positions.
      - Aggregate analysis to compare performance across different circuits, drivers, and constructors.
      - Time-series analysis to track trends across regulation eras.
      - A **Random Forest Regressor** model to predict race finish and determine feature importance.



## Key Findings

### 1\. Qualifying is the Single Strongest Predictor

The data confirms that starting position is paramount. There is a **correlation of 0.82** between qualifying and finishing position. A predictive model explains **52% of the variance (R² = 0.52)** in final race position, with qualifying position being the most important feature.

### 2\. Win & Podium Probabilities Drop Sharply

Starting from pole gives a driver a **\~42% chance of winning** and a **\~65% chance of a podium**. This probability falls dramatically with each position back; a driver starting 5th has only a 4% chance of winning.

### 3\. Circuit Design is Critical for Overtaking

The track layout has a massive impact on position changes. Long straights and heavy braking zones in modern circuits like Baku produce significantly more action than tight, narrow street circuits.

| Circuit Type | Avg Positions Gained | Example Circuits |
| :--- | :--- | :--- |
| High Overtaking | +3.2 positions | Baku, Monza, Spa |
| Medium Overtaking | +1.8 positions | Silverstone, Interlagos |
| Low Overtaking | +0.7 positions | Monaco, Singapore |

### 4\. Regulation Changes Have Increased Action

Modern regulations, particularly the introduction of DRS in the Hybrid Era, have led to a noticeable increase in position changes compared to older eras.

  - **V10 Era (1993-2005):** +1.5 avg. positions gained
  - **V8 Era (2006-2013):** +1.7 avg. positions gained
  - **Hybrid Era (2014-2021):** +2.1 avg. positions gained
  - **Ground Effect Era (2022-Present):** +1.9 avg. positions gained

### 5\. Elite Drivers Consistently Gain Positions

Certain drivers have a proven track record of outperforming their qualifying results over their careers. This analysis required a minimum of 50 classified race finishes for statistical significance.

**Top 5 Drivers by Avg. Positions Gained:**

1.  Fernando Alonso: +2.8
2.  Lewis Hamilton: +2.6
3.  Sergio Perez: +2.5
4.  Michael Schumacher: +2.3
5.  Daniel Ricciardo: +2.2



## Technical Stack

  - **Data Processing:** Python, Pandas, NumPy
  - **Database:** SQLite
  - **Machine Learning:** Scikit-learn
  - **Visualization:** Matplotlib, Seaborn, Plotly
  - **Version Control:** Git/GitHub



## Conclusion

1.  **Qualifying is crucial, but not everything:** While starting position is the strongest predictor, race outcomes are significantly influenced by **circuit design (35%)** and the **regulation era (23%)**.
2.  **Modern F1 has more overtaking:** The Hybrid Era, with its powerful DRS, increased average position changes by **40%** compared to the V10 era.
3.  **Driver skill is a quantifiable advantage:** Top performers like Fernando Alonso consistently gain positions, proving that elite racecraft can overcome a subpar qualifying result.
4.  **Circuit design dictates the show:** A high-speed track like Baku produces over **3 times more** position changes on average than a tight circuit like Monaco.



## Project Links

  - **GitHub Repository:** `https://github.com/thabisondebele/f1_project`

