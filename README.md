# AI News Event Clustering & Timeline Builder

## 📌 Project Overview

In modern media, the same real-world event is reported by hundreds of news articles across different platforms and dates. This makes it difficult to understand:

- What the actual event is  
- How it evolved over time  
- What were the key milestones  

This project builds an **AI-based system** that automatically:
- Groups news articles into real-world events using **unsupervised learning**
- Constructs a **chronological timeline** for each event  
- Generates a **readable event summary**
- Presents results through a **Streamlit dashboard**

The project focuses on:
- Natural Language Processing (NLP)
- Large-scale text processing  
- Unsupervised clustering  
- Event storytelling from data  

---


## 📊 Dataset Details

**Source:** GDELT News Dataset (latest 1 month of data)  
**Initial size:** ~2 million articles  

### Key fields used:
- `date` – Published date  
- `article_title` – Original title  
- `article_content` – Original content  
- `source` – News outlet  
- `url` – Article link  

### Engineered fields created by you:
- `project_title` – Merged and cleaned title representation  
- `project_content` – Combined title + article content  
- `clean_text` – Lemmatized and cleaned text  
- `event_cluster` – Cluster ID from MiniBatch KMeans  
- `event_label` – Human-readable event name  
- `event_summary` – Short event description  
- `is_noise` – Flag for unrelated/noisy articles  
- `year_month` – Temporal grouping feature  

To improve performance, the cleaned dataset was later converted from **CSV → Parquet**.

---

## 🏗️ System Architecture (Your exact pipeline)

### **1) Data Ingestion & Basic Cleaning**
You:
- Downloaded **latest 1 month GDELT data**
- Selected a **2 million article sample**
- Standardized column names  
- Handled missing values  
- Converted dates to proper datetime format  

---

### **2) Title & Content Engineering (Key Innovation)**
You discovered an important issue:
- Many article titles were **too similar**, causing poor labeling and duplicate keywords.

To fix this, you:
- Combined **source article content + title** into:
  - `project_title`
  - `project_content`
- This created richer, more meaningful text for clustering.

---

### **3) Text Preprocessing**
You performed:
- Lowercasing  
- Removing special characters  
- Stopword removal  
- Lemmatization  
- Created a clean textual feature: `clean_text`

This improved semantic consistency before vectorization.

---

### **4) Text Representation (Vectorization Choice)**
You experimented with:
- Bag of Words  
- Word2Vec (word embeddings)  
- Sentence embeddings  

However, **they crashed due to 2M rows + memory limits.**

Final practical decision:
👉 **HashingVectorizer**  

Reasons:
- Memory efficient  
- Works well with very large datasets  
- No need to store full vocabulary  
- Stable for millions of documents  

---

### **5) Event Clustering (Core AI Task)**

You used:
- **Mini-Batch KMeans**  

Why?
- Scales well to large datasets  
- Faster than standard KMeans  
- Lower memory usage  
- Suitable for sparse text vectors  

You determined the best number of clusters using:
- **Elbow Method**

Then you:
- Assigned each article an `event_cluster`
- Identified and removed noisy/unrelated clusters using:
  - Very small cluster sizes  
  - Irrelevant keyword patterns  

---

### **6) Event Labeling**
For each cluster, you:
- Extracted top keywords  
- Checked most frequent sources  
- Reviewed dominant themes  
- Assigned a meaningful label such as:
  - “World Economic Forum in Davos (2026-01)”  
  - “US–Venezuela Relations (2026-01)”  
  - “India Current Affairs (2026-01)”

Stored as: `event_label`

---

### **7) Timeline Construction**
For each event cluster:
- Articles were sorted by `date`
- You built a temporal sequence showing:
  - Event start  
  - Major developments  
  - Latest updates  

This was later visualized in your dashboard.

---

### **8) Event Summary Generation**
For each cluster, you generated:
- Short readable summary like:

> “This event began on 2025-12-25, saw major developments in January 2026, and was widely covered by international media.”

Stored as: `event_summary`

---

### **9) User Interface — Streamlit Dashboard**
Your final output includes a **Streamlit dashboard** that shows:
- List of detected events (cluster dropdown)
- Timeline visualization for top 3 events  
- Event summaries  
- Option to click and read articles inside each event  

---

## ▶️ How to Run the Project

### **Step 1 — Install dependencies**
```bash
pip install -r requirements.txt

### **Step 2 — (Optional) Convert CSV to Parquet**

python convert_to_parquet.py

### **Step 3 — Run dashboard**

streamlit run NLP.py


## ⚠️ Challenges Faced

- **Very large dataset (2 million rows)**  
  - Caused crashes in Word2Vec and embeddings  
  - Solution: Used **HashingVectorizer + MiniBatch KMeans**

- **Poor labeling due to duplicate titles**  
  - Fixed by merging article title with article content  

- **Slow loading in Streamlit**  
  - Solved by converting CSV → Parquet  

- **Noise in clustering**  
  - Handled by filtering small or irrelevant clusters  


  ## Sample Outputs

### 1️⃣ Dashboard Home View  
![Dashboard](outputs/dashboard_screenshot_1.png)

### 2️⃣ Event Summary View  
![Event Summary](outputs/dashboard_screenshot_2.png)

### 3️⃣ Timeline Visualization  
![Timeline](outputs/dashboard_screenshot_3.png)

### 4️⃣ Read Articles Panel  
![Articles](outputs/dashboard_screenshot_4.png)

### 5️⃣ Event Timeline Table  
![Timeline Table](outputs/dashboard_screenshot_5.png)

# AI-News-Event-Clustering
# AI-News-Event-Clustering
