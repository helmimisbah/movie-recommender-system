# 🎬 Movie Recommender System  
**Collaborative Filtering · Content-Based · Hybrid Recommender**

A production-oriented movie recommender system built on the MovieLens dataset.  
This project focuses on **scalability, memory efficiency, and realistic engineering decisions**, rather than toy examples or full-matrix approaches.

---

## 📌 Project Overview

This project implements three recommendation strategies:

1. **Collaborative Filtering (CF)** using Implicit ALS  
2. **Content-Based Recommendation** using TF-IDF + cosine similarity  
3. **Hybrid Recommender System** combining CF and Content-Based signals  

The system is designed to:
- Handle large-scale interaction data
- Avoid memory-heavy full prediction matrices
- Generate recommendations **on-the-fly**
- Be extensible toward production deployment

---

## 📊 Dataset

**Source:** [MovieLens  ](https://www.kaggle.com/datasets/parasharmanas/movie-recommendation-system)

- Users: ~160K  
- Movies: ~30K+  
- Ratings: ~25M interactions  

### Data Files
- `ratings_for_cf.parquet`  
  - `userId`, `movieId`, `rating`
- `movies.csv`  
  - `movieId`, `title`, `genres`

---

## 🧠 Methods

### 1️⃣ Collaborative Filtering

**Algorithm:** Alternating Least Squares (ALS)  
**Library:** `implicit`  

**Key Characteristics:**
- Sparse user–item matrix (CSR)
- Item–user matrix used for ALS training
- No full user–item prediction matrix
- Recommendations generated per user on demand

**Why ALS?**
- Scalable for large datasets
- Efficient with sparse implicit feedback
- Widely used in industry-grade recommender systems

---

### 2️⃣ Content-Based Recommendation

**Features:**
- Movie title
- Movie genres  

**Technique:**
- TF-IDF vectorization
- Cosine similarity (computed on-the-fly)

A content-based similarity function (`content_similar_base`) retrieves movies that are semantically similar to a given seed movie.

**Purpose:**
- Enable cold-start recommendations
- Provide semantic similarity independent of user interactions
- Support hybrid recommendation logic

---

### 3️⃣ Hybrid Recommender System

The final recommendation score is computed as:

Hybrid Score = α × CF Score + (1 − α) × Content-Based Score

Where:
- **α (alpha)** controls personalization vs content similarity
- Default value: `α = 0.7`

**Design Philosophy:**
- Preserve strong collaborative signals
- Enrich results using content similarity
- Avoid overriding CF when interaction data is strong
- Improve robustness for sparse metadata and cold-start cases

---

## ⚙️ Key Engineering Decisions

- Avoided full user–item and similarity matrices to prevent memory issues
- Used sparse CSR matrices for scalability
- Generated recommendations on-the-fly
- Explicitly handled missing metadata (`Unknown Title`, `Unknown Genres`)
- Designed hybrid logic to enrich, not override, collaborative filtering

---

## ⚠️ Limitations

- Some movies lack complete metadata, limiting content-based signals
- Hyperparameters (ALS factors, regularization, hybrid alpha) can be further tuned
- Evaluation is offline; no online A/B testing

---

## 🚀 Future Work

- Refactor logic into reusable Python modules (`app/`)
- Serve recommendations via REST API (FastAPI)
- Add model persistence & caching
- Deploy as a microservice
- Implement online evaluation and monitoring