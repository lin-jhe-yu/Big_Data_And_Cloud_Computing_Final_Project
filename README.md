<img width="800" height="450" alt="Image" src="https://github.com/user-attachments/assets/d1ca36e8-8cff-4534-a70a-fda99b3882ab" />


# Reviews to Risk: Modeling Business Closure and Neighborhood Ecosystems

## Executive Summary

Restaurant closures represent a significant economic and operational risk in the U.S. food services industry. With **17–30% of restaurants failing in their first year** and nearly **50% closing within five years**, operators, investors, and cities face substantial losses from sunk capital, disrupted employment, and weakened neighborhood ecosystems. Although online reviews influence **90% of customers**, they are seldom used in a structured, decision-focused manner.

### Business Impact

This project shows how advanced analytics can convert existing data into **tangible business value**:

* **Earlier intervention:** A high-performing closure prediction model (**AUC ≈ 0.97**) enables stakeholders to identify at-risk restaurants well before permanent shutdown, creating time for corrective action.
* **Higher success rates for new openings:** Location and neighborhood analysis improves site selection decisions, increasing the likelihood of long-term survival in competitive markets.
* **Operational performance gains:** Benchmarking against features present in **75%+ of high-performing restaurants** provides a clear, data-backed playbook for improving resilience and customer appeal.

### Solution

We developed a **scalable decision-support and early warning platform** that integrates:

* **Predictive machine learning** to estimate the probability of permanent restaurant closure
* **Natural language processing (NLP)** to translate millions of unstructured customer reviews into actionable performance signals
* **Graph analytics** to assess neighborhood strength, competitive pressure, and ecosystem health

Applied across **California, New York, and Illinois**, the solution supports:

* **Restaurant operators** seeking proactive risk monitoring and operational guidance
* **Entrepreneurs and investors** evaluating market entry and location strategy
* **Urban planners and analysts** monitoring commercial stability at the neighborhood level

Overall, the approach shifts decision-making from reactive responses to closures toward **proactive, data-driven risk management and growth strategy**.

### Technical Overview

* **Data Storage & Processing:** Google Cloud Storage (GCS)  
* **Scalable Data Pipelines:** PySpark for ingestion and transformation  
* **Machine Learning:** Spark MLlib for predictive modeling  
* **Text & Sentiment Analysis:** Transformer-based models (BERT) for processing customer reviews  
* **Graph Analytics:** Assessing neighborhood networks, competitive pressure, and ecosystem health  

---

## Business Problem

Restaurants operate on thin margins and face intense competition, especially in dense urban markets. Traditional metrics (e.g., ratings alone) often fail to capture early signs of decline.

We address three core questions:

1. **Can customer reviews reveal early signals of operational or performance risk?**
2. **Which business, pricing, and neighborhood factors most strongly predict permanent closure?**
3. **How can these insights be translated into practical guidance for opening or stabilizing restaurants in CA, NY, and IL?**

---

## Data Overview (What We Analyzed)

* **Geography:** California, New York, Illinois
* **Data Sources:**

  * Restaurant business metadata (location, price tier, amenities, closure status)
  * Customer reviews and star ratings
* **Scale & Reality:**

  * ~1 GB business metadata
  * ~39 GB review text data
  * Over **171K restaurants** and **52M+ reviews** processed for analysis.  

**Why this matters:** The volume and diversity of data require scalable engineering solutions and ensure findings reflect real market conditions rather than small-sample bias.

---

## Key Business Insights

### 1. Reviews Matter — More Than Just Ratings

* Average ratings are strong predictors of restaurant survival, but the true value of reviews lies in the detailed and actionable insights they provide.
* Reviews reveal operational challenges such as slow service, inconsistent quality, or limited offerings that are not apparent from ratings or sentiment alone.

### 2. Pricing Strategy Impacts Risk

* **Low-priced restaurants** have the lowest closure rate (~8%)
* Mid-range and expensive restaurants face significantly higher operational risk

### 3. Location and Competition Are Critical

* Closures cluster in major urban hubs such as **New York, Chicago, and Los Angeles**
* High-density restaurant markets increase both opportunity and failure risk

### 4. What Successful Restaurants Have in Common

Restaurants in top-performing neighborhoods tend to share:

* Casual, cozy atmosphere
* Takeout and delivery options
* Features supporting **groups, families, and solo dining**
* Accessibility (e.g., wheelchair access, high chairs)

These features appear in **75%+ of well-performing businesses**, making them strong operational benchmarks.

---

## How the Solution Works (Technical Details That Matter)

### 1. Sentiment Modeling (Customer Voice at Scale)

**Business Goal:** Convert millions of unstructured customer reviews into **1 to 5 sentiment scores**.  

* Implemented using **PySpark** for scalability
* Data preparation:

  * Removed duplicates
  * Filtered to English-language reviews
    
**Sentiment Score Prediction Models:**  
We evaluated several predictive models using **TF-IDF** features and **BERT embeddings** (`small_bert_L4_512`). Customer review ratings were used as a proxy for sentiment scores. Model performance was assessed using standard metrics: **RMSE**, **MAE**, and **R²**.  

| Model Type               | Feature Type | RMSE  | MAE  | R²   |
|---------------------------|-------------|-------|------|------|
| **Linear Regression**     | TF-IDF      | 0.99  | 0.75 | 0.40 |
|                           | BERT        | 0.89  | 0.67 | 0.51 |
| **Gradient Boosted Trees**| TF-IDF      | 1.03  | 0.78 | 0.34 |
|                           | BERT        | 0.94  | 0.67 | 0.46 |
| **Random Forest Regressor** | TF-IDF    | 1.13  | 0.89 | 0.21 |
|                           | BERT        | 1.00  | 0.77 | 0.39 |

> **Key Insight:** BERT embeddings consistently outperform TF-IDF features, capturing richer semantic context. The **Linear Regression + BERT** model achieved the highest R² (**0.51**), making it the best performing model for sentiment score prediction.


**Why this approach:** Lightweight BERT balances interpretability, cost, and performance for large-scale deployment.

---

### 2. Restaurant Closure Prediction (Early Risk Detection)

**Business Goal:** Predict the probability that a restaurant will permanently close, enabling proactive intervention months before closure.

#### Feature Groups
- **Operational attributes:** Amenities, dining options
- **Customer feedback metrics:** Ratings, sentiment score, review volume
- **Pricing and population context**

#### Model Performance
| Model | AUC | Accuracy | F1 Score | Precision | Recall |
|-------|-----|----------|----------|-----------|--------|
| Logistic Regression | 0.959 | 0.894 | 0.904 | 0.933 | 0.894 |
| Gradient Boosted Trees (GBT) | 0.972 | 0.912 | 0.920 | 0.942 | 0.912 |
| Random Forest | 0.971 | 0.908 | 0.917 | 0.942 | 0.908 |

**Best Model:** Gradient Boosted Trees (GBT) — **AUC ≈ 0.972**

#### Feature Importance (GBT)

| Feature                                      | GBT Importance | Effect Direction               |
|---------------------------------------------|----------------|-------------------------------|
| Popular for Solo dining                      | 0.44           | ↓ Strong Survival Driver      |
| Service options Takeout                      | 0.12           | ↓ Strong Survival Driver      |
| num of reviews                               | 0.09           | ↓ Strong Survival Driver      |
| Atmosphere Casual                            | 0.05           | ↓ Strong Survival Driver      |
| Amenities Good for kids                       | 0.04           | ↑ Strong Failure Driver       |
| Service options Dine-in                       | 0.03           | ↓ Strong Survival Driver      |
| Offerings Comfort food                        | 0.03           | ↑ Weak Failure Driver         |
| Accessibility Wheelchair accessible entrance | 0.03           | ↓ Strong Survival Driver      |
| price numeric                                | 0.02           | ↑ Weak Failure Driver         |
| Planning Accepts reservations                | 0.02           | ↑ Weak Failure Driver         |
| Payments Debit cards                          | 0.01           | ↑ Strong Failure Driver       |
| Offerings Quick bite                          | 0.01           | ↓ Strong Survival Driver      |
| Payments NFC mobile payments                  | 0.01           | ↓ Strong Survival Driver      |
| avg rating                                   | 0.01           | ↓ Weak Survival Driver        |
| Dining options Lunch                          | 0.01           | ↑ Weak Failure Driver         |

**Interpretation:**  
The model can reliably distinguish high-risk vs. low-risk restaurants. Features like *Popular for Solo dining* and *Takeout service options* are strong survival indicators, while *Price numeric* and *Planning Accepts reservations* are associated with higher failure risk. This enables actionable early interventions.

---

### 3. Neighborhood & Ecosystem Analysis (Graph Analytics)

**Business Goal:** Understand how neighborhood dynamics influence restaurant success and identify high-performing similar businesses for benchmarking.

---

### 3.1 Neighborhood Influence Graph

**Graph Type:** Bipartite graph (Businesses ↔ Neighborhoods)

**Purpose:** Identify neighborhoods that disproportionately contribute to business resilience.

#### Edge Construction
Edge weights capture performance and survival signals:

- Average rating  
- Review volume  
- Sentiment score  
- Survival probability  

**Edge Weight:**  
- **Weight A:** `avg_rating * sqrt(review_count) * (1 - closure_prob)`  
- **Weight B:** `0.4 * shrunk_sentiment + 0.3 * rating + 0.3 * survival`

#### Analysis Method
- Apply **PageRank** to identify high-influence neighborhoods  
- Highlight neighborhoods connected to resilient businesses

<img width="600" height="350" alt="Image" src="https://github.com/user-attachments/assets/a0b1f73c-c6e6-47cd-9bae-e54e4c2d5fdd" />

**Key Insight:**  
- Location decisions should consider ecosystem health, not just foot traffic.

---

### 3.2 Business Similarity Graph

**Purpose:** Identify structurally and performance-wise similar businesses.

#### A. Operational Similarity
- Binary operational attributes (e.g., `Atmosphere_Casual`, `Takeout`, `Solo_dining`)  
- Similarity metric: `1 - MinHash distance`

#### B. Performance Similarity
- Similarity score: `0.3 * (1 - Δavg_success_score) + 0.3 * (1 - Δavg_sentiment) + 0.4 * (1 - Δavg_rating)`  
where `Δ` denotes normalized differences.
- Recommendation Score: `0.3 * PageRank + 0.5 * avg_success_score + 0.2 * normalized_total_degree`

<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/01c163a2-afc7-437d-9908-4298348d24e8" />

**Key Insight:**  
- Operationally similar businesses cluster together, revealing common service models.  
- Performance similarity identifies high-performing peers with comparable ratings and sentiment.  
- Combining PageRank and performance metrics yields a robust recommendation score for benchmarking and improvement.

---

## Business Recommendations

* **For New Restaurants:**

  * Target high-PageRank neighborhoods in cities like **New York, Chicago, and Los Angeles**
  * Adopt widely successful operational features early

* **For Existing Restaurants:**

  * Monitor drops in high-importance predictors (ratings, solo dining popularity, takeout)
  * Treat declining sentiment trends as early warning signals

---

## Engineering & Scalability

* **Frameworks:** PySpark, Spark MLlib
* **Storage:** Parquet with Snappy compression
* **Execution Model:** DAG-based workflows
* Designed to handle **tens of millions of reviews** efficiently

---

## Limitations & Assumptions

* Potential bias from fake or paid reviews
* Sentiment scores assume ratings align with textual sentiment
* ML models trained on subsets due to data scale
* Lightweight BERT chosen for efficiency over maximum accuracy

---

## Future Business Value

* **Proactive Risk Alert System:** Real-time monitoring for at-risk restaurants
* **Feature Expansion:** Integrate operating costs and maintenance data
* **Geo-Based Warnings:** Detect unstable neighborhoods using graph signals

---

## Team member

Aarav Dewangan · Akshaj Chandwani · Dhruvi Gandhi · Lawrence Lin · Szuyu Chi
