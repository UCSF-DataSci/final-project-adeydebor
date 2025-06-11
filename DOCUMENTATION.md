# Predicting Viral Music Success: A Machine Learning Analysis

## Project Overview

This project uses machine learning to predict viral success in music based on cross-platform engagement metrics accumulated over time. **Using engagement metrics collected between song release dates and December 2024, we analyze whether accumulated platform activity can identify which songs achieved viral status.** By analyzing 4,389 songs from Spotify's most-streamed tracks, we developed predictive models that achieve 92.1% accuracy in identifying which songs achieved viral status (>232M streams by December 31, 2024).

## Team Members

- Deborah Adeyemi

## Problem Statement

The music industry struggles to predict which songs will achieve viral success, leading to inefficient marketing spend and missed opportunities. This project addresses the question: **Can we identify viral music success using accumulated engagement metrics across streaming platforms between release and December 2024?**

**Key Questions:**
- What accumulated engagement factors most strongly identify viral success?
- How important is time since release in explaining viral achievement?
- Can cross-platform engagement patterns accumulated over time identify songs that achieved viral status?

## Dataset Description

**Source:** [Kaggle - Most Streamed Spotify Songs 2024](https://www.kaggle.com/datasets/nelgiriyewithana/most-streamed-spotify-songs-2024)

**Dataset Characteristics:**
- **Size:** 4,488 songs initially, 4,389 after cleaning (99 removed due to missing essential data)
- **Time Period:** Songs released from 1930-2024, with majority from 2015-2024
- **Target Variable:** Binary viral classification (>232M streams = viral, median threshold)
- **Features:** 18 original features reduced to 13 modeling features after multicollinearity analysis

**Input Features:**
- **Spotify Metrics:** Playlist Count, Playlist Reach, Popularity, Streams
- **Cross-Platform:** YouTube Views, TikTok Posts/Likes, Apple Music Playlists
- **Discovery:** Shazam Counts, YouTube Playlist Reach
- **Song Characteristics:** Track Score, All Time Rank, Explicit Track
- **Temporal:** Days Since Release, Release Year, Release Month

**Data Quality:**
- **Missing Data:** 21-25% missing for TikTok metrics, 22% for YouTube Playlist Reach
- **Imputation Strategy:** Median imputation for numerical features
- **Viral Distribution:** 50% viral rate (balanced classification)

## Tools and Methods

### **Programming Environment:**
- **Language:** Python 3.x
- **Key Libraries:** scikit-learn, pandas, numpy, matplotlib, seaborn
- **Platform:** Jupyter Notebook

### **Machine Learning Pipeline:**
1. **Data Preprocessing:**
  - **Stage 1:** Missing value imputation (median strategy)
  - **Stage 2:** Automated duplicate removal (same track + artist combinations)
  - Feature selection based on correlation analysis
  - Multicollinearity removal (correlation threshold >0.8)

2. **Feature Engineering:**
  - Binary viral classification using median streams threshold
  - Standardization for logistic regression
  - Temporal feature creation (Days Since Release)

3. **Model Development:**
  - **Logistic Regression:** Baseline interpretable model
  - **Random Forest:** Ensemble method for capturing non-linear patterns
  - 80/20 train-test split with stratified sampling

4. **Model Evaluation:**
  - 5-fold cross-validation for robustness testing
  - ROC-AUC analysis for discrimination ability
  - Feature importance analysis for business insights

## Key Decisions and Trade-offs

### **1. Data Deduplication Strategy (Two-Stage Process)**
- **Stage 1 - Manual ISRC Cleaning:** Used ISRC codes to identify and remove incorrect duplicate entries before Python analysis
- **Stage 2 - Automated Python Deduplication:** Systematic removal of true duplicates (same track + same artist) while preserving legitimate different versions
- **Rationale:** Ensures data integrity while maintaining different versions of songs by different artists
- **Process:** 
 - Manual: ISRC-based duplicate identification and removal
 - Automated: Keep highest-streaming version when same track name + same artist appear multiple times
- **Trade-off:** Required both manual and automated steps but ensured comprehensive duplicate handling

### **2. Viral Threshold Selection**
- **Decision:** Used median streams (232M) as viral threshold
- **Rationale:** Created balanced classification while maintaining meaningful business distinction
- **Trade-off:** Somewhat arbitrary cutoff, but provides clear binary classification

### **3. Feature Selection Strategy**
- **Decision:** Removed highly correlated features (TikTok Views, YouTube Likes)
- **Rationale:** Eliminated multicollinearity while retaining predictive power
- **Trade-off:** Lost some potentially useful information but improved model stability

### **4. Missing Data Handling**
- **Decision:** Median imputation for 21-25% missing TikTok/YouTube data
- **Rationale:** Simple, robust method that preserves sample size
- **Trade-off:** May mask important patterns in missing data mechanism

### **5. Model Selection**
- **Decision:** Chose Random Forest as primary model
- **Rationale:** Superior performance (92.1% vs 88.3% accuracy) and feature importance insights
- **Trade-off:** Less interpretable than logistic regression but better predictive power

## Issues Overcome

### **1. Data Preprocessing and Deduplication (Two-Stage Process)**
- **Issue:** Original dataset contained both incorrect duplicate entries and legitimate different versions of songs
- **Solution:** Implemented comprehensive two-stage deduplication strategy
- **Stage 1 - Manual ISRC Cleaning:**
 - Exported dataset to Excel for manual review
 - Used ISRC (International Standard Recording Code) as unique song identifier
 - Identified songs with same ISRC but different metadata entries
 - Removed duplicate entries while preserving most complete/accurate records
- **Stage 2 - Automated Python Deduplication (Section 2.3):**
 - Identified potential duplicates by track name using `duplicated()` function
 - Distinguished between legitimate different versions (same track, different artists) and true duplicates (same track + same artist)
 - Applied strategy: Keep highest-streaming version for true duplicates, preserve all legitimate different versions
 - Systematic removal using `drop_duplicates(subset=['Track', 'Artist'], keep='first')` after sorting by streams
- **Outcome:** Clean dataset with accurate unique song entries and preserved legitimate song variations

### **2. Data Quality Challenges**
- **Issue:** Significant missing data in TikTok metrics (21-25%)
- **Solution:** Implemented robust median imputation and analyzed impact on results
- **Outcome:** Maintained 4,389 songs (97.8% retention rate)

### **3. Multicollinearity Problems**
- **Issue:** High correlations between similar platform metrics (r=0.99 for TikTok Views/Likes)
- **Solution:** Systematic correlation analysis and strategic feature removal
- **Outcome:** Reduced from 18 to 13 features while maintaining predictive power

### **4. Class Balance Considerations**
- **Issue:** Need for meaningful viral classification threshold
- **Solution:** Tested multiple thresholds, selected median for balanced classes
- **Outcome:** 50/50 viral distribution enabling robust binary classification

### **5. Model Interpretability vs Performance**
- **Issue:** Tension between interpretable linear models and complex ensemble methods
- **Solution:** Developed both logistic regression and Random Forest models
- **Outcome:** Combined interpretable insights with superior predictive performance

## How to Run the Code

### **Prerequisites:**
```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
```

## Repository Structure:
viral-music-prediction/
├── README.md
├── PROJECT_DOCUMENTATION.md
├── viral_music_analysis.ipynb
├── requirements.txt
├── data/
│   └── spotify_data.csv

### **Execution Steps:**
1. **Data Preprocessing (Manual):**
   - Download original dataset from Kaggle
   - Open in Excel/Google Sheets for manual cleaning
   - Use ISRC codes to identify and remove duplicate entries
   - Save cleaned dataset as CSV for Python analysis

2. **Clone Repository:**
   ```bash
   git clone [repository-url]
   cd viral-music-prediction
   ```
3. **Install Dependencies:**
  ```bash
  pip install -r requirements.txt
  ```
4. **Launch Jupyter Notebook:**
  ```bash
  jupyter notebook viral_music_analysis.ipynb
  ```
5. **Run Analysis:**
  - Execute cells sequentially from Section 1 through Section 8
  - Section 2.3 includes automated duplicate detection and removal
  - Each section builds on previous results
  - Total runtime: approximately 5-10 minutes

### **Expected Outputs (Displayed in Notebook):**
All results are displayed inline within the Jupyter notebook cells:
- Data exploration statistics and visualizations
- Correlation matrix heatmap
- Model performance metrics (accuracy, AUC, confusion matrices)
- ROC curves comparison
- Feature importance plots and coefficients
- Top 10 viral songs analysis with streaming statistics

## Example Output

### **Model Performance Summary:**
*See notebook output for detailed performance tables and visualizations*

### **Key Results:**
- **Random Forest:** 92.1% test accuracy, AUC = 0.983
- **Logistic Regression:** 88.3% test accuracy, AUC = 0.952  
- **Cross-validation:** Random Forest (0.980 ± 0.006), Logistic Regression (0.956 ± 0.015)

### **Top Predictive Features:**
1. **Spotify Playlist Count** (28.2% importance) - Dominant factor for viral success
2. **Days Since Release** (13.8% importance) - Timing is critical for viral momentum
3. **Spotify Playlist Reach** (12.7% importance) - Broader playlist exposure matters

### **Business Insights:**
- Songs with higher Spotify playlist inclusion have **46× higher viral odds**
- Newer releases have **5× higher viral probability** than older songs
- Cross-platform presence significantly boosts viral potential

### **Visualizations Available in Notebook:**
- Correlation matrix heatmap showing feature relationships
- ROC curves comparing model performance
- Feature importance plots (both logistic regression odds ratios and Random Forest importance)
- Distribution plots of key variables
- Top 10 viral songs analysis

## Key Findings and Conclusions

### **Primary Discovery: Spotify Playlist Dominance**
Our analysis reveals that **accumulated Spotify playlist placement is by far the strongest identifier of viral success**, with an odds ratio of 46.5 in logistic regression and 28.2% feature importance in Random Forest. This suggests that cumulative playlist exposure over time either drives viral success or serves as a strong indicator of viral potential.

### **Timing is Everything**
**Days Since Release** emerges as the second most important factor, with songs having more time to accumulate engagement showing higher viral achievement rates. This indicates that viral success often requires sustained engagement accumulation over extended periods.

### **Cross-Platform Strategy Wins**
Songs that accumulated engagement across multiple platforms (Spotify + Apple Music + Shazam) between release and December 2024 show significantly higher viral achievement probability, suggesting that diversified engagement accumulation strategies outperform single-platform focus.

### **TikTok Surprise**
Despite TikTok's reputation for viral content, our analysis shows accumulated TikTok metrics have modest negative effects on viral identification, suggesting viral songs may gain TikTok traction after achieving virality through other channels, or that TikTok engagement doesn't necessarily translate to streaming platform success.

### **Business Applications**
With 92.1% prediction accuracy, this retrospective analysis demonstrates that accumulated engagement patterns could provide significant value for:
- **Record Labels:** Understanding which engagement accumulation patterns lead to viral success
- **Artists:** Focusing on sustained engagement building across platforms over time
- **A&R Teams:** Identifying songs with viral achievement patterns for strategic analysis
- **Streaming Platforms:** Understanding how platform engagement contributes to long-term viral success

### **Model Robustness**
The consistency between logistic regression and Random Forest models, combined with strong cross-validation performance (AUC = 0.980 ± 0.006), provides high confidence in these retrospective findings for understanding viral achievement patterns.

### **Limitations and Future Work**
**Current Limitations:**
- **Missing Data Impact:** TikTok metrics (21-25% missing), YouTube Playlist Reach (22% missing) were median-imputed
- **Temporal Snapshot:** Analysis based on cumulative metrics rather than growth trajectories
- **Platform Evolution:** Engagement patterns may shift as algorithms and user behavior evolve
- **Causation vs Correlation:** Cannot establish whether playlist placement drives virality or vice versa

**Future Research Directions:**
- **Time-Series Analysis:** Track engagement velocity and growth patterns over time
- **Causal Inference:** Use quasi-experimental methods to establish causal relationships
- **Real-Time Prediction:** Build models for early viral detection (7-30 days post-release)
- **Advanced Modeling:** Explore ensemble methods, XGBoost, or neural networks for pattern recognition
- **Audio Features:** Incorporate musicological features (tempo, key, energy) alongside engagement metrics

## Citations and References

### **Data Sources:**
- Nelgiriyewithana. (2024). *Most Streamed Spotify Songs 2024*. Kaggle. https://www.kaggle.com/datasets/nelgiriyewithana/most-streamed-spotify-songs-2024

### **Technical References:**
- Pedregosa, F., et al. (2011). Scikit-learn: Machine learning in Python. *Journal of Machine Learning Research*, 12, 2825-2830.
- McKinney, W. (2010). Data structures for statistical computing in Python. *Proceedings of the 9th Python in Science Conference*, 56-61.
- Hunter, J. D. (2007). Matplotlib: A 2D graphics environment. *Computing in Science & Engineering*, 9(3), 90-95.

### **Methodological References:**
- Breiman, L. (2001). Random forests. *Machine Learning*, 45(1), 5-32.
- Hosmer Jr, D. W., Lemeshow, S., & Sturdivant, R. X. (2013). *Applied logistic regression* (Vol. 398). John Wiley & Sons.
- Fawcett, T. (2006). An introduction to ROC analysis. *Pattern Recognition Letters*, 27(8), 861-874.

_Claude AI and ChatGPT were used to assist in developing this code._
