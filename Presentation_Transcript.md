# Presentation Transcript — AAI-590 Capstone
### Season & Soil-Based Crop Planning with Price-Aware Profitability Forecasting
**Target length: ~6 minutes** (individual project range: 3–7 min)

---

## Slide 1 — Title
*(~15 seconds)*

"Hi, I'm Ankur Bhagat, and this is my AAI-590 capstone project: Season and Soil-Based Crop Planning with Price-Aware Profitability Forecasting — a machine learning decision support tool for farmers in the Vidarbha/Chandrapur cotton belt of Maharashtra, India."

---

## Slide 2 — The Problem
*(~40 seconds)*

"Farmers in this region face two connected decisions every season that are usually treated separately. First: what to plant, given their soil and the coming season's weather. Second: when to sell, once market prices start moving.

Existing tools only solve half the problem. Soil-based crop recommendation systems suggest an agronomically suitable crop, but ignore expected market return. Price forecasting tools help with sell-timing, but assume the crop choice has already been made.

My contribution is a two-module system that connects both decisions — so a farmer can weigh agronomic suitability *alongside* expected profitability, specific to the Vidarbha/Chandrapur belt."

---

## Slide 3 — Data Sources
*(~45 seconds)*

"For crop recommendation, I used the Kaggle Crop Recommendation dataset — 2,200 records, 22 crops, 100 each, with seven features: nitrogen, phosphorus, potassium, temperature, humidity, pH, and rainfall. No missing values, confirmed by direct inspection.

For price forecasting, I used two real datasets. My primary dataset is genuinely region-specific: monthly Soybean prices for Chandrapur district itself, from Government of Maharashtra mandi data — 26 real months, aggregated across all reporting APMCs in the district. As a secondary, larger-sample robustness check, I also used daily Agmarknet Soybean price data from Sehore, Madhya Pradesh — 364 days — to see whether my modeling approach behaves consistently with more training data than Chandrapur alone provides."

---

## Slide 4 — Methods
*(~50 seconds)*

"For crop recommendation, I trained a feed-forward neural network — two hidden layers of 64 and 32 nodes with ReLU activation — alongside a Random Forest baseline for comparison. Both used a 70/15/15 stratified train/validation/test split with Adam and early stopping.

For price forecasting, I used a feed-forward neural network on sliding windows of price history, compared against a differenced autoregression baseline. Both series used a chronological split, since this is time-series data — no shuffling, so the test set reflects genuinely unseen future dates.

One honest note: TensorFlow and Keras weren't available in my development sandbox, so I implemented both neural networks using scikit-learn — genuine backpropagation-trained networks, just not the exact library. Keras templates for exact reproduction are included in the notebook."

---

## Slide 5 — Results: Crop Recommendation
*(~40 seconds)*

"[Show Figure 1: training loss and validation accuracy curves]

The MLP achieved 98.79% test accuracy, with a macro F1 of 0.988, converging smoothly in 53 iterations — no overfitting or underfitting. The Random Forest baseline did slightly better, at 99.70%.

Only 4 misclassifications out of 330 test samples — and all four were agronomically sensible, like jute confused with rice, or muskmelon with watermelon — not random noise. Feature importance also independently reproduced a published finding from Gupta et al. 2022: rainfall and humidity as the top two predictors."

---

## Slide 6 — Results: Price Forecasting
*(~55 seconds)*

"[Show Figures 2 and 3: Chandrapur and Sehore price series]

This is where I want to be transparent rather than favorable. On my primary, on-scope Chandrapur series — only 26 months, 5 held out for testing — a naive persistence baseline, just predicting next month equals this month, actually beat both my trained models. With only 5 test points, this isn't evidence either model is flawed — it's an honest finding about the limits of modeling on this little real data.

On the larger secondary Sehore series — 364 days, 55-day test set — both trained models beat the naive baseline, with the autoregression model slightly ahead of the neural network. That's the reverse of what published literature typically finds at larger scale, which I attribute to this still being a single-market series rather than a multi-year, multi-market dataset."

---

## Slide 7 — Conclusion & Next Steps
*(~45 seconds)*

"So — does combining crop recommendation with price forecasting produce a more useful planning tool? The crop recommendation half is strongly supported: both models are highly accurate on the exact dataset used in prior published work. The price-forecasting half now has a genuinely real, on-scope result, even though the small Chandrapur sample means I can't yet recommend it for real farmer-facing use.

The most interesting outcome wasn't just that the models worked — it's that my Random Forest's feature importance ranking independently reproduced a published result, despite a completely different train/test split. That kind of replication gives real confidence.

Going forward: combine both modules into the single profitability-ranking output originally proposed, reproduce both neural networks in Keras or PyTorch for full compliance, and keep accumulating real Chandrapur-specific price history as it becomes available. Thank you."

---

## Notes for recording
- **Total estimated time:** ~5.5 minutes at a natural pace — comfortably within the 3–7 minute window.
- **Visuals required:** minimum 3. This script assumes at least 3 (Figure 1 training curves, Figure 2 Chandrapur, Figure 3 Sehore) — your current deck is missing these entirely and needs to be rebuilt to match.
- If you want to trim to ~4 minutes, the safest cuts are: the "honest note" about scikit-learn vs. Keras in Slide 4, and shortening the Sehore explanation in Slide 6 to one sentence.
- If you want to stretch toward 7 minutes, consider adding: a brief mention of the end users (smallholder farmers, extension officers) from your Introduction, or a sentence on the Ramakumar et al. (2017) cost-and-risk-sensitivity finding from your Literature Review as future-work context.
