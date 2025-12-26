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

Overall, the platform shifts decision-making from reactive responses to closures toward **proactive, data-driven risk management and growth strategy**.

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

### 1. Reviews Matter — But Not in the Obvious Way

* Average star rating has **more influence on survival** than raw sentiment alone
* Many closed restaurants share operational issues, such as slow serving speed

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

**Business goal:** Convert millions of unstructured customer reviews into **1 to 5 sentiment scores**.  

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

## 2. Restaurant Closure Prediction (Early Risk Detection)

**Business Goal:** Predict the probability that a restaurant will permanently close, enabling proactive intervention months before closure.

### Feature Groups
- **Operational attributes:** Amenities, dining options
- **Customer feedback metrics:** Ratings, sentiment score, review volume
- **Pricing and population context**

### Model Performance

| Model                        | AUC  | Accuracy | F1 Score | Precision | Recall |
|-------------------------------|------|---------|----------|-----------|--------|
| Logistic Regression           | 0.96 | 0.89    | 0.90     | 0.93      | 0.89   |
| Gradient Boosted Trees (GBT)  | 0.97 | 0.91    | 0.92     | 0.94      | 0.91   |
| Random Forest                 | 0.97 | 0.91    | 0.92     | 0.94      | 0.91   |

**Best Model:** Gradient Boosted Trees (GBT) — **AUC ≈ 0.97**

### Feature Importance (GBT)

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
The model can reliably distinguish high-risk vs. low-risk restaurants. Features like *Popular for Solo dining* and *Takeout service options* are strong survival indicators, while amenities for kids or debit card payments are associated with higher failure risk. This enables actionable early interventions.

---

### 3. Neighborhood & Ecosystem Analysis (Graph Analytics)

**Business goal:** Understand how neighborhood dynamics influence restaurant success.

* Bipartite graph: **Businesses ↔ Neighborhoods**
* Edge weights combine:

  * Ratings
  * Review volume
  * Sentiment
  * Survival probability
* PageRank identifies **high-influence neighborhoods** linked to resilient businesses

**Outcome:** Location decisions can be informed not just by foot traffic, but by ecosystem health.

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

## Team

**Group 5**
Aarav Dewangan · Akshaj Chandwani · Dhruvi Gandhi · Lawrence Lin · Szuyu Chi

---

## Academic Context

Developed as part of the **M.S. in Applied Data Science** program, emphasizing real-world decision-making, scalable analytics, and business impact.
