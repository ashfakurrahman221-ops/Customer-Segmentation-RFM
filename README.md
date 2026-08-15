# Customer-Segmentation-RFM
Customer Segmentation using RFM analysis and KMeans Clustering
# 🛍️ Customer Segmentation using RFM Analysis & KMeans Clustering

An unsupervised machine learning project that segments retail customers into **Diamond**, **Gold**, and **Silver** tiers based on their purchasing behavior, using **RFM (Recency, Frequency, Monetary)** analysis combined with **KMeans clustering**.

---

## 🎯 Problem Statement

Not all customers are equally valuable to a business. Treating every customer the same wastes marketing budget and misses opportunities to retain high-value buyers or win back inactive ones. This project analyzes historical transaction data to automatically group customers by their purchasing behavior, so a business can target each segment with the right strategy — retention for the best customers, re-engagement for the rest.

---

## 📂 Dataset

- **Source**: [Online Retail Dataset](https://archive.ics.uci.edu/dataset/352/online+retail) — UCI Machine Learning Repository (also hosted on [Kaggle](https://www.kaggle.com/datasets/rahmanashfakur/onlineretail))
- **Citation**: Chen, D. (2015). Online Retail [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5BW33
- **Raw size**: 541,909 transactions across 8 columns (`InvoiceNo`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `UnitPrice`, `CustomerID`, `Country`)

---

## 🧹 Data Cleaning

Before RFM features can be built, the following issues were removed from the raw data:

| Issue | Action |
|---|---|
| Missing `CustomerID` | Dropped (RFM cannot be computed without a customer) |
| Cancelled invoices (`InvoiceNo` starting with "C") | Removed |
| Negative `Quantity` (returns) | Removed |
| `UnitPrice` = 0 | Removed |
| Duplicate rows | Removed |

**Result:** 541,909 → **392,692 rows** (27.5% removed), covering **4,338 unique customers**.

---

## ⚙️ RFM Feature Engineering

For each customer, three metrics were computed relative to a reference date (last invoice date + 1 day, i.e. **2011-12-10**):

- **Recency** — days since the customer's most recent purchase
- **Frequency** — number of distinct invoices (orders) placed
- **Monetary** — total amount spent (`Quantity × UnitPrice`, summed)

| Metric | Mean | Std | Min | Median | Max |
|---|---|---|---|---|---|
| Recency (days) | 92.5 | 100.0 | 1 | 51 | 374 |
| Frequency (orders) | 4.3 | 7.7 | 1 | 2 | 209 |
| Monetary ($) | 2,048.7 | 8,985.2 | 3.75 | 668.6 | ~280,206 |

The RFM values were **standardized** (`StandardScaler`) before clustering, since KMeans is distance-based and Monetary's much larger scale would otherwise dominate Recency/Frequency.

---

## 📈 Choosing the Number of Clusters

- **Elbow Method**: SSE (inertia) plotted for k = 1–9; the drop in SSE clearly flattens around **k = 3**, indicating diminishing returns beyond that point.
- **Silhouette Score validation** across k = 2–7 confirmed k=3 as a strong, interpretable choice for producing meaningful business-relevant segments (rather than picking the single mathematically highest score, which occurred at k=2 with only two broad groups).

## 🤖 KMeans Clustering (k = 3)

Customers were clustered into 3 groups, then ranked and labeled by average Monetary value:

| Segment | Customers | Avg. Recency | Avg. Frequency | Avg. Monetary |
|---|---|---|---|---|
| 💎 **Diamond** | 26 | 6.0 days | 66.4 orders | $85,826 |
| 🥇 **Gold** | 3,230 | 41.5 days | 4.7 orders | $1,850 |
| 🥈 **Silver** | 1,082 | 247.1 days | 1.6 orders | $630 |

**Total: 4,338 customers** segmented.

---

## ✅ Clustering Quality (Validation)

Since this is **unsupervised learning** with no ground-truth labels, classification metrics like accuracy/F1 don't apply. Instead, clustering-specific metrics were used:

- **Silhouette Score**: **0.594** (closer to 1 = well-separated, well-formed clusters)
- **Davies–Bouldin Index**: **0.710** (closer to 0 = less cluster overlap)

Both indicate reasonably well-separated, meaningful clusters.

---

## 📊 Visualizations

- Elbow curve (SSE vs. k)
- Silhouette score comparison across k = 2–7
- Customer count by segment (bar chart)
- Recency / Frequency / Monetary distribution histograms

---

## 💡 Key Insights & Business Recommendations

- **Diamond (26 customers)** — Extremely high spend & order frequency, very recent activity. These are the business's most valuable customers → prioritize **retention**: loyalty programs, exclusive offers, dedicated support.
- **Gold (3,230 customers)** — The bulk of the customer base, moderate activity → good target for **targeted promotions** to increase purchase frequency and move them toward Diamond status.
- **Silver (1,082 customers)** — Long time since last purchase, low order frequency → highest **churn risk**; suited for **win-back campaigns** (discounts, reminder emails).

---

## 🛠️ Tech Stack

**Language:** Python

**Libraries:** Pandas, NumPy, Scikit-learn (`StandardScaler`, `KMeans`, `silhouette_score`, `davies_bouldin_score`), Matplotlib, Seaborn

---

## 📁 Project Structure

```
Customer-Segmentation-RFM/
│
├── RFM_Customer_Segmentation.ipynb   # Full pipeline: cleaning → RFM → scaling → clustering → evaluation
└── README.md                          # Project documentation
```

---

## ▶️ How to Run

1. Clone the repository
   ```bash
   git clone https://github.com/ashfakurrahman221-ops/Customer-Segmentation-RFM.git
   cd Customer-Segmentation-RFM
   ```
2. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/rahmanashfakur/onlineretail) or [UCI](https://archive.ics.uci.edu/dataset/352/online+retail) and place `OnlineRetail.csv` in the project folder
3. Open and run the notebook
   ```bash
   jupyter notebook RFM_Customer_Segmentation.ipynb
   ```

---

## 👤 Author

**Ashfakur Rahman**
📧 ashfakurrahman221@gmail.com

---

## 📄 License

This project is open-source and available for educational purposes.
