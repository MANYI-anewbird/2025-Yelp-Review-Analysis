# 2025 Yelp Review Analysis

**The question:** When someone asks a real question about a café — drink quality, noise, wifi — why do the first reviews they see still fail to answer it?

Default Yelp sort is recency and stars. That is a ranking for *browsing*, not for *deciding*. This BA820 (Section A, Team 10) study takes the public [Yelp Open Dataset](https://www.yelp.com/dataset), cuts it to cafés so the text methods can actually run, and builds a **preference → relevant reviews** retrieve: given a `business_id` and a question, return the reviews that speak to that question, with topic and sentiment attached.

**How we built that:** **GCP** scale-up (chunked JSON → Parquet), then **TF-IDF + NMF** topics and a **stars-supervised sentiment** model, wired into a preference ranker (`get_relevant_review`).

Proposal: [`docs/proposal.pdf`](docs/proposal.pdf). Final write-up: [`docs/final.pdf`](docs/final.pdf). Group archive of [BARATZL/Text_mining_reviews](https://github.com/BARATZL/Text_mining_reviews).

---

## Decision this supports

Two users, one failure mode:

1. **A guest** — “Is the coffee actually good?” should not have to scroll past five generic five-stars.
2. **An operator** — if you only read five-star blurbs, you systematically miss the complaints that predict churn.

The work is built so a preference string (e.g. drink quality) can pull the *right* reviews, not the *latest* ones.

---

## What we found

Started from ~**8 million** reviews on a GCP VM (JSON → Parquet, ~8.65 GB → ~5 GB). Full-corpus TF-IDF and UMAP did not fit in memory; the **café / Starbucks slice** is the working set — that is a scope choice, not a footnote.

**Negative reviews were almost invisible to the first classifier.** On the café sentiment model, rebalancing lifted **negative-review recall from 0.04 to 0.42**. Positive F1 stayed ~0.92. In plain terms: before the fix, the model was a five-star detector. After, it can still find the 1–2 star complaints an operator should read first.

**Topics that actually separate on cafés:** NMF with **4 topics** distributed more cleanly than LDA on this slice (drink / service / place / wait-style themes — see notebook 06). Retrieval then ranks by semantic closeness to the preference, not by star sort.

**What we would not claim:** this is not a production search engine, and it is not trained on all 8M reviews. The 8M pass was for scale and EDA; the decision tool lives on the café subset.

---

## How we got there (short)

| Step | Why it mattered |
|---|---|
| Chunked JSON → Parquet on GCP | 8M rows had to become a table before any text work |
| Café subset | Full-corpus TF-IDF / UMAP killed the kernel (`docs/notebook-04.md`) |
| TF-IDF, then NMF (4 topics) | LDA mixed; NMF was the readable topic cut |
| Stars + TextBlob, then rebalance | Catch the negatives the raw split ignored |
| `get_relevant_review(business_id, preference)` | The only output a guest or operator would actually use |

---

## What’s in the repo

Run notebooks in order. Point them at a local or GCS copy of the academic dump — Parquet/JSON files are not in git.

| Notebook | Role |
|---|---|
| [`01_Preprocessing`](notebooks/01_Preprocessing_sectionA_team10.ipynb) | GCS pull, chunked JSON → Parquet |
| [`02_EDA`](notebooks/02_EDA_sectionA_team10.ipynb) | Length, stars, cities, power users |
| [`03_appliedmethod`](notebooks/03_appliedmethod_sectionA_team10.ipynb) | K-means / PCA on numeric features |
| [`05_Text_Mining`](notebooks/05_Text_Mining_sectionA_team10.ipynb) | Café tokenize, BoW, TF-IDF |
| [`06_Topic_Modeling`](notebooks/06_Topic_Modeling_sectionA_team10.ipynb) | LDA → NMF |
| [`07_Sentiment_Analysis`](notebooks/07_Sentiment_Analysis_sectionA_team10.ipynb) | TextBlob + star-label split |
| [`08_Functions`](notebooks/08_Functions_sectionA_team10.ipynb) | `get_relevant_review` |

`docs/team-log.md` is the week-by-week working log.

---

## How we built it (technical)

Stack: **GCP VM** + **PyArrow Parquet** → **NLTK** tokenize → **scikit-learn** TF-IDF / NMF / classifiers → **TextBlob** polarity → a thin retrieval function.

**Scale.** Full dump ~8M reviews. Chunked JSON reads on GCS, JSON → Parquet (~8.65 GB → ~5 GB). EDA and numeric clustering (K-means; PCA to 6 components ≈ 85% variance, then cluster again) ran on that table. **UMAP on the full corpus killed the kernel.** Full-corpus TF-IDF was too expensive (`docs/notebook-04.md`). The working NLP set is the **café subset**.

**Text.** Tokenize, stopwords, punctuation; bag-of-words then **TF-IDF**. Topics: **LDA first, then NMF (k = 4)** — NMF’s loadings were the ones we could name (drink / service / place / wait). We did not keep LDA as the production cut.

**Sentiment.** Two labels: TextBlob polarity, and a **stars-supervised** split. The raw classifier treated almost every review as positive (**negative recall 0.04**). Class rebalancing (not a new architecture) moved negative recall to **0.42**; positive F1 stayed ~0.92. That is the technical proof behind “the first model was a five-star detector.”

**Retrieval.** `get_relevant_review(business_id, preference)` embeds the preference against the café-review TF-IDF space, returns nearest reviews, and attaches the NMF topic + sentiment. It is cosine-style ranking on a fitted vectorizer — not a separate LLM.

**What we left on the table.** No transformer embeddings, no production index. The strength on display is *making 8M rows tractable, then picking the subset and the model that still answer a guest’s question.*

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
