Sephora Product Value Score (PVS) Project

1. Objective
Sephora carries thousands of products across multiple categories (Skincare, Makeup, Fragrance, etc.). The business question is:
  Which categories and products provide the most value to customers and to Sephora’s business?
Since we don’t have direct sales data, we design a Product Value Score (PVS) that combines key signals of product success:
Customer engagement (number of reviews)
Price (revenue per unit)
Customer sentiment (ratings)

3. Methodology
   
Step 1: Data Preparation
Cleaned the dataset to retain relevant columns:
 brand, category, name, rating, number_of_reviews, price, MarketingFlags, exclusive, limited_edition, limited_time_offer
Converted numerical columns (price, rating, reviews) to numeric values.
Encoded binary features (exclusive, MarketingFlags, etc.) as 0/1.

Step 2: Feature Importance (SHAP)
Trained an XGBoost regressor with a proxy target:
 Y = price × number_of_reviews 
Used SHAP (SHapley values) to identify feature importances.

Findings:
number_of_reviews (≈ 70%) — strongest predictor
price (≈ 28%) — secondary factor
rating (≈ 0.7%) — small influence
Marketing flags, exclusives, limited editions — negligible impact

Step 3: Defining PVS
Based on SHAP importances, we built a PVS formula using only the top 3 drivers:
PVS = 0.7046 ⋅ f(reviews) + 0.2818 ⋅ f(price) + 0.0070 ⋅ f(rating)  
Where:
f(reviews) = log⁡(1+reviews) / max⁡log⁡(1+reviews)
f(price) = price / max⁡(price)​
f(rating)=rating / 5​
The result is normalized to a 0–100 scale for comparability.

Step 4: Ranking
Ranked Top 15 categories by total reviews and product count.
Ranked Top 15 products by PVS.
Compared Top 15 by reviews vs Top 15 by product count to highlight mismatches (e.g., categories with many products but little engagement).

Step 5: Visualization
Histogram → PVS distribution across all products.
Bar charts → Top 15 categories (average PVS) and Top 15 products (by PVS).
Comparison plot → Product count vs total reviews per category.

3. Tools Used
Python (Anaconda, Jupyter Notebook)

Libraries:
pandas → data cleaning, aggregation
numpy → transformations, log-scaling
matplotlib → visualization
xgboost → model training
shap → feature importance & explainability

5. Results & Insights
Customer Engagement is King 
Reviews dominate the PVS (70%). Categories with high engagement drive value more than categories with just many products.
Price Amplifies Value 
Expensive products rank higher if they also have engagement.
Luxury categories like skincare and fragrance show high PVS.
Ratings Matter Less 
Ratings influence PVS only slightly. A 4.1★ product with 10,000 reviews is more valuable than a 5★ product with 20 reviews.
Marketing Flags & Exclusives Don’t Move the Needle 
Special labels have negligible impact compared to organic engagement.

6. Case Study Example
A product with 11 reviews, price $53, rating 4.5 had low PVS, despite being marketed — reviews dragged down the score.


A product with 2,000 reviews, price $40, rating 4.2 ranked among the Top 10 products due to massive engagement.
