# 🎌 Anime Recommender System

A personalized anime recommendation system that implements and compares two ML approaches: **Content-Based Filtering** and **Collaborative Filtering**. Built with Python and Streamlit.

---

## 🚀 Features

- **Content-Based Filtering** — recommends anime similar in genre/type to titles you liked
- **Collaborative Filtering** — recommends based on patterns from 500 simulated users
  - Item-Based: finds anime with correlated rating patterns
  - User-Based: finds users with similar taste and aggregates their preferences
- **Interactive UI** with real-time filtering by genre and type
- **Model comparison** — overlap analysis and genre distribution charts
- Custom dark anime-themed Streamlit interface

---

## 📁 Project Structure

```
anime_recommender/
├── app.py                    # Main Streamlit application
├── requirements.txt
├── data/
│   ├── generate_dataset.py   # Dataset generation script
│   ├── anime.csv             # 107 anime titles with genres/ratings
│   └── ratings.csv           # 11,162 synthetic user ratings
├── models/
│   ├── content_based.py      # Content-Based Filtering (cosine similarity)
│   └── collaborative.py      # Collaborative Filtering (item-based & user-based)
└── utils/
    ├── data_loader.py         # Data loading and preprocessing
    └── evaluation.py          # Model comparison utilities
```

---

## ⚙️ Setup & Run

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/anime-recommender.git
cd anime-recommender
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Generate the dataset
```bash
python data/generate_dataset.py
```

> **Note:** To use the real MAL dataset, download it from [Kaggle](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database) and replace `anime.csv` and `ratings.csv` in the `data/` folder.

### 4. Run the app
```bash
streamlit run app.py
```

Open your browser at `http://localhost:8501`

---

## 🧪 Example Usage

1. In the sidebar, select an anime from the dropdown (e.g. *Frieren: Beyond Journey's End*)
2. Set your rating with the slider (1–10)
3. Click **➕ Add**
4. Repeat for 2–5 more titles
5. Optionally set genre/type filters
6. Click **🚀 Get Recommendations**

**Example input:**
| Anime | Your Rating |
|---|---|
| Frieren: Beyond Journey's End | 10 |
| Hunter x Hunter (2011) | 9 |
| Steins;Gate | 10 |

**Content-Based output** — similar genres (Adventure, Fantasy, Drama):
- Vinland Saga, Mushishi, Made in Abyss, ...

**Collaborative output** — what users with similar taste watched:
- Legend of the Galactic Heroes, Code Geass, Monster, ...

---

## 🤖 Models

### Content-Based Filtering

```
Anime → Genre/Type features → MultiLabelBinarizer + OneHot
                                         ↓
                               Cosine Similarity Matrix
                                         ↓
User ratings (weighted) → Profile vector → Top-N similar unseen anime
```

**Key implementation:**
- Genres encoded via `MultiLabelBinarizer` (multi-hot)
- Type encoded via one-hot (`get_dummies`)
- User profile = weighted average similarity, weight = rating / 10

### Collaborative Filtering

```
All ratings → User-Item Matrix (500 users × 107 anime)
                      ↓
           Item-Item Cosine Similarity
           User-User Cosine Similarity
                      ↓
        Weighted aggregation → Predicted scores
```

**Item-Based:** for each rated anime, find items other users also rated similarly  
**User-Based:** build virtual user profile, find K nearest neighbors (K=20), aggregate their ratings

| | Content-Based | Item CF | User CF |
|---|---|---|---|
| Basis | Anime features | Rating correlations | User similarity |
| Cold start (new anime) | ✅ Works | ❌ Needs ratings | ❌ Needs ratings |
| Serendipity | ❌ Similar only | ✅ Can diverge | ✅ Can diverge |
| Speed | Fast | Fast (precomputed) | Fast (precomputed) |

---

## 📊 Dataset

The synthetic dataset models realistic MAL behavior:

| Metric | Value |
|---|---|
| Anime titles | 107 |
| Simulated users | 500 |
| Total ratings | ~11,000 |
| Matrix sparsity | ~79% |
| Rating range | 1–10 |

Anime ratings are sampled from Gaussian noise around real MAL scores to simulate authentic user behavior.

To use real MAL data: [Kaggle — Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database)

---

## 🛠 Tech Stack

- **Python 3.10+**
- **pandas** — data loading, manipulation
- **NumPy** — matrix operations
- **scikit-learn** — MultiLabelBinarizer, cosine_similarity
- **Streamlit** — web interface

---

## 📈 Portfolio Value

This project demonstrates:
- ✅ Real ML system design (two distinct algorithms)
- ✅ Data preprocessing pipeline
- ✅ User-Item matrix construction
- ✅ Similarity-based recommendation
- ✅ Streamlit production-ready UI
- ✅ Model comparison methodology

---

## 📄 License

MIT
