# Tech ABSA Dataset

An Indonesian tech-product review dataset for **Aspect-Based Sentiment Analysis
(ABSA)**, labelled using a weakly-supervised top-down approach.

Each row pairs a review with a single aspect and the sentiment expressed toward
that aspect. A single review may appear in multiple rows, once per detected
aspect.


## Scema

| Column | Description |
|--------|-------------|
| `raw_review` | Original review text (Indonesian). |
| `rating` | Star rating given by the reviewer (1–5). |
| `aspect` | Aspect category (weakly-supervised label). One of: `quality`, `delivery`, `service`, `price`, `general`. |
| `sentiment` | Sentiment toward the aspect (weakly-supervised label). One of: `positive`, `negative`. |

## Statistics

- **Rows (review–aspect pairs):** 7,611
- **Unique reviews:** 5,741

### Sentiment distribution

| Sentiment | Rows |
|-----------|------|
| positive | 5,665 |
| negative | 1,946 |

## License

Released under the [MIT License](LICENSE).
