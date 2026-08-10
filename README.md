# Season & Soil-Based Crop Planning with Price-Aware Profitability Forecasting

This project is a part of the AAI-590 course in the Applied Artificial Intelligence Program at the University of San Diego (USD).

-- Project Status: **Active**

## Installation

1. Clone this repository:
   ```
   git clone https://github.com/abhagatsandiego/AAI-590-Capstone-Season-Soil-Based-Crop-Planning-with-Price-Aware-Profitability-Forecasting.git
   cd AAI-590-Capstone-Season-Soil-Based-Crop-Planning-with-Price-Aware-Profitability-Forecasting
   ```
2. Open `AAI590_Capstone_Notebook_Bhagat.ipynb` in Google Colab (recommended) or Jupyter Notebook locally.
3. Required Python packages: `pandas`, `numpy`, `scikit-learn`, `matplotlib`. All are pre-installed in Colab; for a local environment: `pip install pandas numpy scikit-learn matplotlib`.
4. The notebook expects three data files to be uploaded at their respective upload-prompt cells:
   - `Crop_recommendation.csv` → Section 1 (crop recommendation module)
   - `agmarknet_india_historical_prices_2024_2025.csv` → Section 7 (Sehore, Madhya Pradesh secondary dataset)
   - `Monthly_data_cmo.csv` → Section 7b (Chandrapur, Maharashtra primary dataset)
5. Run all cells top to bottom. Each section is self-contained and labeled (see Project Description below).
6. `train_crop.py` can also be run standalone (`python train_crop.py`) to reproduce the crop recommendation results independent of the notebook; it expects `Crop_recommendation.csv` in the same directory.

**Note on deep learning library:** TensorFlow/Keras were unavailable in the development sandbox used for this project, so neural networks were implemented via scikit-learn's `MLPClassifier`/`MLPRegressor` (genuine backpropagation-trained networks). Keras-equivalent templates are included directly in the notebook (Section 4b) for exact reproduction where TensorFlow is available.

## Project Intro/Objective

The main purpose of this project is to connect two decisions that Vidarbha/Chandrapur farmers currently have to make separately: **what to plant**, based on soil and seasonal conditions, and **when to sell**, based on market price movement. Existing tools address only one half of this problem — soil-based crop recommendation systems ignore expected market return, while price-forecasting tools assume the crop choice has already been made. This project builds a two-module machine learning system that recommends viable crops from soil/climate data and separately forecasts crop prices from real regional market data, so that farmers, and the agricultural extension officers who advise them, can eventually weigh agronomic suitability alongside expected profitability rather than treating the two as independent decisions.

## Partner(s)/Contributor(s)

Individual project — Ankur Uttam Bhagat.

## Methods Used

- Machine Learning (classification, regression)
- Deep Learning (feed-forward neural networks)
- Data Visualization
- Data Cleaning / Exploratory Data Analysis
- Time-Series Forecasting

## Technologies

- Python
- scikit-learn
- pandas / NumPy
- Matplotlib
- Jupyter Notebook / Google Colab

## Project Description

This project uses three real datasets across two modules:

**Module 1 — Crop Recommendation:** The Kaggle Crop Recommendation dataset (`Crop_recommendation.csv`, Ingle, n.d.) — 2,200 records, 22 crop classes (100 each), 7 numeric features (nitrogen, phosphorus, potassium, temperature, humidity, pH, rainfall), no missing values. A feed-forward neural network (MLP, two hidden layers of 64/32 nodes) and a Random Forest baseline were trained on a 70/15/15 stratified split. The MLP reached 98.79% test accuracy (macro F1 = 0.988); the Random Forest reached 99.70%. Feature importance (rainfall, humidity as top predictors) independently reproduced a published finding (Gupta et al., 2022) despite a completely different train/test split.

**Module 2 — Price Forecasting:** Two real price datasets. The primary, on-scope dataset (`Monthly_data_cmo.csv`) is genuine Chandrapur district, Maharashtra Soybean price data — 26 real months (Sept 2014–Oct 2016), aggregated across 8 reporting APMCs — sourced from Government of Maharashtra mandi data. A secondary, larger-sample robustness check (`agmarknet_india_historical_prices_2024_2025.csv`) uses 364 days of Agmarknet Soybean price data from Sehore, Madhya Pradesh. A feed-forward neural network and a differenced-autoregression baseline were trained on both series with a chronological (no-shuffle) split. On the small primary Chandrapur sample (n=5 test points), a naive persistence baseline outperformed both trained models — reported honestly as a limitation of the small sample size rather than reframed favorably. On the larger secondary Sehore sample (n=55), both trained models beat the naive baseline, with the autoregression baseline slightly ahead of the neural network.

**Roadblocks/challenges:** TensorFlow/Keras were unavailable in the development sandbox (addressed via scikit-learn equivalents, see Installation). Region-specific Maharashtra price data was difficult to source — the initial Agmarknet export did not cover Maharashtra, so a Madhya Pradesh proxy was used as a secondary check while a genuine Maharashtra-specific dataset (Government of Maharashtra mandi data via the SocialCops Data Science Challenge dataset) was located and used as the primary series instead.

**Remaining work:** combine the two modules into the single profitability-ranking output originally proposed; reproduce both neural networks in Keras/PyTorch using the templates already included in the notebook; continue accumulating real Chandrapur-specific price history to strengthen the price-forecasting evaluation.

See the [full report](./Bhagat_Ankur_AAI590_Final_Report.pdf) for complete methodology, results, and discussion.

## Repository Contents

| File | Description |
|---|---|
| `AAI590_Group8_Capstone_Project_Notebook_A....ipynb` | Full pipeline: data cleaning, EDA, model design/training/optimization, and analysis for both modules (8 sections) |
| `AAI590_Group8_Capstone_Project_Notebook_A....pdf` | PDF export of the above notebook, with real executed outputs for the crop recommendation module |
| `train_crop.py` | Standalone script reproducing the crop recommendation module (MLP + Random Forest) |
| `Crop_recommendation.csv` | Kaggle crop recommendation dataset |
| `Datasets.zip` | Price-forecasting datasets: Chandrapur (Maharashtra mandi data, primary) and Sehore (Agmarknet, Madhya Pradesh, secondary) |
| `AAI590_Group8_Capstone_Project Report.pdf` | Final written report |
| `AAI590_Group8_Capstone_PPT.pptx` | Final presentation slides |
| `Presentation_Transcript.md` | Speaking script for the recorded presentation |
| `LICENSE` | MIT License |

## License

This project is licensed under the [MIT License](./LICENSE).

## Acknowledgments

AI Disclosure: Claude (Anthropic) was used throughout this project to assist with drafting report prose, organizing citations, and structuring code. All AI-assisted content was reviewed, verified against primary sources, and edited by the author.
