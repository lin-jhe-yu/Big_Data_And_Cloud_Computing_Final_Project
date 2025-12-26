# Reviews to Risk: Modeling Business Closure and Neighborhood Ecosystems

## Executive Summary

Restaurant closures represent a significant economic and operational risk in the U.S. food services industry. With **17–30% of restaurants failing in their first year** and nearly **50% closing within five years**, operators, investors, and cities face substantial losses from sunk capital, disrupted employment, and weakened neighborhood ecosystems. Despite the widespread influence of online reviews—used by **90% of customers**—these signals are rarely leveraged in a systematic, decision-oriented way.

### Estimated Business Impact

This project shows how advanced analytics can convert existing data into **tangible business value**:

* **Earlier intervention:** A high-performing closure prediction model (**AUC ≈ 0.97**) enables stakeholders to identify at-risk restaurants well before permanent shutdown, creating time for corrective action.
* **Financial loss mitigation:** Achieving even a **5–10% reduction in closure rates** in major urban markets could result in **millions of dollars annually** in avoided losses from lease defaults, rehiring costs, and failed capital investments.
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
  * Over **92K restaurants** and **29M+ reviews** in California alone

**Why this matters:** The volume and diversity of data require scalable engineering solutions and ensure findings reflect real market conditions rather than small-sample bias.

---

## Key Business Insights

### 1. Reviews Matter — But Not in the Obvious Way

* Average star rating has **more influence on survival** than raw sentiment alone
* Review length shows **little predictive power** despite closed restaurants having longer reviews
* The *presence and consistency* of feedback is more informative than verbosity

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

**Business goal:** Convert millions of unstructured reviews into reliable business-level signals.

* Implemented using **PySpark** for scalability
* Data preparation:

  * Removed duplicates
  * Filtered to English-language reviews
* Models evaluated:

  * Linear Regression
  * Random Forest Regressor
  * Gradient Boosted Trees Regressor
  * Lightweight BERT (`small_bert_L4_512`, R² ≈ 0.51)

**Why this approach:** Lightweight BERT balances interpretability, cost, and performance for large-scale deployment.

---

### 2. Restaurant Closure Prediction (Early Risk Detection)

**Business goal:** Predict the probability that a restaurant will permanently close.

* Feature groups:

  * Operational attributes (amenities, dining options)
  * Customer feedback metrics (ratings, sentiment, review volume)
  * Pricing and population context
* Best model:

  * **Gradient Boosted Trees (GBT)**
  * **AUC ≈ 0.97**

**Interpretation:** The model can reliably distinguish high-risk vs. low-risk restaurants, enabling proactive intervention months before closure.

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
