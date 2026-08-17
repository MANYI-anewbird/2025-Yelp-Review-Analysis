# Yelp Review Scout

Find the reviews that actually answer your question — not just the most recent five-star blurbs.

BA820 (Section A, Team 10) NLP project on the public [Yelp Open Dataset](https://www.yelp.com/dataset). We started from the full ~8M-review dump on GCP, then narrowed to cafés so TF-IDF, topic models, and sentiment could actually run. The last notebook turns that pipeline into a **preference → relevant reviews** helper (`get_relevant_review`).

This is Manyi’s archive of the group homework. Original repository: [BARATZL/Text_mining_reviews](https://github.com/BARATZL/Text_mining_reviews).

---

## What we built

1. **Scale first** — chunked JSON reads on a GCP VM, JSON → Parquet (~8.65 GB → ~5 GB).
2. **EDA** — review length vs stars, power users, city/business distributions.
3. **Numeric clustering** — K-means, then PCA (6 components ≈ 85% variance) before clustering again. Full-corpus UMAP kept killing the kernel.
4. **Text mining** — tokenize / stopwords / punctuation; BoW then TF-IDF. Full 8M reviews was too expensive (see `docs/notebook-04.md`); café subset in notebook 05 is the working version.
5. **Topics** — LDA then **NMF** (4 topics), which distributed more cleanly on the café/Starbucks slice.
6. **Sentiment** — TextBlob polarity plus a stars-supervised split.
7. **Retrieval** — given a `business_id` and a preference string (e.g. drink quality), return semantically close reviews with topic + sentiment attached.

On the café sentiment classifier, rebalancing lifted **negative-review recall from 0.04 to 0.42** (positive F1 stayed ~0.92).

---

## Notebooks

Run in order. Point them at a local or GCS copy of the academic dataset — Parquet/JSON dumps are not in this repo.

| Notebook | |
|---|---|
| [`01_Preprocessing_sectionA_team10.ipynb`](notebooks/01_Preprocessing_sectionA_team10.ipynb) | GCS pull, chunked JSON → Parquet |
| [`02_EDA_sectionA_team10.ipynb`](notebooks/02_EDA_sectionA_team10.ipynb) | Exploratory plots |
| [`03_appliedmethod_sectionA_team10.ipynb`](notebooks/03_appliedmethod_sectionA_team10.ipynb) | K-means / PCA clustering on numeric features |
| [`05_Text_Mining_sectionA_team10.ipynb`](notebooks/05_Text_Mining_sectionA_team10.ipynb) | Café-subset tokenization, BoW, TF-IDF |
| [`06_Topic_Modeling_sectionA_team10.ipynb`](notebooks/06_Topic_Modeling_sectionA_team10.ipynb) | LDA → NMF |
| [`07_Sentiment_Analysis_sectionA_team10.ipynb`](notebooks/07_Sentiment_Analysis_sectionA_team10.ipynb) | TextBlob + star-label split |
| [`08_Functions_sectionA_team10.ipynb`](notebooks/08_Functions_sectionA_team10.ipynb) | `get_relevant_review(business_id, preference)` |

`docs/proposal.pdf` is the original team proposal. `docs/team-log.md` is the week-by-week working log.

---

## Setup

```sh
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Place `reviews2.parquet`, `users2.parquet`, `business.parquet`, and (for later notebooks) `cafe_reviews.parquet` next to the notebooks, or edit the read paths.

---

## License

MIT. See [LICENSE](LICENSE).
