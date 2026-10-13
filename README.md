# Tech ABSA Dataset

An Indonesian tech-product review dataset for **Aspect-Based Sentiment Analysis
(ABSA)**, labelled using a weakly-supervised top-down approach.

Each row pairs a review with a single aspect and the sentiment expressed toward
that aspect. A single review may appear in multiple rows, once per detected
aspect.


## Schema

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

## Reproducibility

The `reproducibility/` folder contains complete weak-label rules, joint aspect/sentiment counts, and the fixed experiment partitions:

- `weak_label_rules.json`: preprocessing, keyword/cue lists, aspect selection, and sentiment decision order.
- `counts_by_aspect_sentiment.csv`: counts for each aspect and sentiment in train, validation, and test.
- `split_ids.csv`: 5,491 training rows, 848 validation rows, and 850 test rows, in the original experiment order.

### Split references

| Column | Meaning |
|---|---|
| `partition` | `train`, `validation`, or `test`. |
| `split_row_index` | Zero-based position within the partition; preserves experiment and prediction order. |
| `row_index` | Zero-based data-row reference into `tech_topdown_absa_dataset.csv`, excluding its header. |
| `source` | Historical source alias such as `tech_1`; not a buyer identifier. |
| `aspect` | Target aspect used by the experiment, including manually corrected evaluation aspects. |
| `sentiment` | Weak training label or manually verified validation/test label. |

`row_index` replaces scraped review IDs. It points to the first public dataset row belonging to that original review. Every aspect pair from the same original review uses this same canonical reference, so review grouping is preserved even though the dataset contains multiple aspect rows per review. Retrieve review text from the referenced row, but use the target aspect and sentiment from `split_ids.csv`; they need not match the weak aspect/sentiment of that canonical dataset row.

The train/validation/test partitions contain 4,283/705/688 distinct canonical review references, with no overlap between partitions. Validation and test retain 13 and 17 repeated review-aspect targets from the published evaluation. Do not deduplicate or reorder these rows when reproducing the main results. Identifier separation does not guarantee content separation: identical short review texts can occur under different canonical references.

For model inputs, apply the released preprocessing rules to the referenced `raw_review` and use `aspek: {aspect} ulasan: {clean_text}`. One original validation input, at `split_row_index=752`, had an empty stored `clean_text` despite nonempty raw text. Preserve an empty review-text component for that row to reproduce the recorded validation setup. Training and test input text otherwise matches normalization of the referenced public reviews.

Row references depend on this dataset's exact row order. SHA-256 of `tech_topdown_absa_dataset.csv`:

```text
6c277c3a224b027752141ca95f25156e7c4e7506fa49859f537aaee81750571b
```

Do not sort or replace the dataset CSV without regenerating its row references. The split export contains no scraped review IDs; the original experimental source artifacts remain separate from this public release.
