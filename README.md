# Tech Top-Down ABSA Dataset

An Indonesian tech-product review dataset for **Aspect-Based Sentiment Analysis
(ABSA)**, labelled using a weakly-supervised top-down approach.

Each row pairs a review with a single aspect and the sentiment expressed toward
that aspect. A single review may appear in multiple rows, once per detected
aspect.

## File

| File | Description |
|------|-------------|
| `tech_topdown_absa_dataset.csv` | The dataset (7,611 rows). |
| `build_dataset.py` | Script used to derive the CSV from the weakly-supervised labels. |

## Schema

| Column | Description |
|--------|-------------|
| `raw_review` | Original review text (Indonesian). |
| `rating` | Star rating given by the reviewer (1–5). Empty for 1,295 rows whose source rating was unavailable. |
| `aspect` | Aspect category (weakly-supervised label). One of: `quality`, `delivery`, `service`, `price`, `general`. |
| `sentiment` | Sentiment toward the aspect (weakly-supervised label). One of: `positive`, `negative`. |

## Statistics

- **Rows (review–aspect pairs):** 7,611
- **Unique reviews:** 5,741

### Aspect distribution

| Aspect | Rows |
|--------|------|
| quality | 3,319 |
| delivery | 1,744 |
| general | 1,120 |
| price | 987 |
| service | 441 |

### Sentiment distribution

| Sentiment | Rows |
|-----------|------|
| positive | 5,665 |
| negative | 1,946 |

### Rating distribution

| Rating | Rows |
|--------|------|
| 5 | 3,821 |
| (missing) | 1,295 |
| 1 | 981 |
| 3 | 726 |
| 2 | 428 |
| 4 | 360 |

## Labelling method

Labels were produced with a **weakly-supervised, top-down** pipeline. Aspects
were assigned first (`keyword_match`, `general_opinion`, or
`fallback_uncovered`), then sentiment was scoped to each aspect
(`aspect_scope`). Because the labels are machine-generated rather than
human-annotated, they may contain noise.

## Reproducing the CSV

The script reads the weakly-supervised labels and the rating source files. If
those live in a parent workspace (with `labels/` and `Dataset/` folders), point
the script at them:

```bash
ABSA_CSV=/path/to/labels/topdown_tech_absa.csv \
SOURCE_GLOB="/path/to/Dataset/*_cleaned.csv" \
python3 build_dataset.py
```

This writes `tech_topdown_absa_dataset.csv` next to the script.

## License

Released under the [MIT License](LICENSE).
