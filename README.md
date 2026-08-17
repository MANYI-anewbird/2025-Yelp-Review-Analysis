# Yelp Review Text Mining

BA820 team project (Section A, Team 10). NLP on the public [Yelp Open Dataset](https://www.yelp.com/dataset): preprocessing at scale, EDA, TF-IDF / text mining, topic modeling (NMF / LDA), sentiment, and a similarity-based review summary.

This copy is Manyi’s archive of the group homework. Original repository: [BARATZL/Text_mining_reviews](https://github.com/BARATZL/Text_mining_reviews).

---

## Notebooks

| File | |
|---|---|
| `01_Preprocessing_sectionA_team10.ipynb` | JSON → Parquet, chunked reads on GCP |
| `02_EDA_sectionA_team10.ipynb` | Exploratory analysis |
| `03_appliedmethod_sectionA_team10.ipynb` | Applied method / first analysis |
| `05_Text_Mining_sectionA_team10.ipynb` | Text mining |
| `06_Topic_Modeling_sectionA_team10.ipynb` | Topic models |
| `07_Sentiment_Analysis_sectionA_team10.ipynb` | Sentiment |
| `08_Functions_sectionA_team10.ipynb` | Shared helpers (including similarity summary) |

The Yelp JSON/Parquet dumps are not in this repo (too large). Point the notebooks at a local or GCS copy of the academic dataset.

---

## License

MIT. See [LICENSE](LICENSE).
