# Goodreads Data Engineering 

## Overview
This lab performs a full data cleaning and feature engineering process on Goodreads review data, following the same logic as implemented in Microsoft Fabric.

---

## Cleaning Steps
- Removed rows with missing `rating`, `review_text`, `book_id`, or `author_id`.
- Removed duplicate reviews by `review_id` and `(user_id, book_id)`.
- Normalized text fields (trim, lowercase, remove malformed characters).
- Dropped reviews with very short text.
- Corrected data types and validated numeric ranges.
- Dropped irrelevant columns.

---

## Feature Engineering
- Added `review_length_words`.
- Aggregated by `book_id` to compute:
  - `avg_rating`
  - `num_reviews`
- Saved the enriched dataset in Delta format under `gold/features_v1`.

---

## Output Path
`abfss://lakehouse@goodreadsreviews60106541.dfs.core.windows.net/gold/features_v1/`

---

## Verification
- Schema and record count inspected successfully in Spark:
  ```python
  df_check = spark.read.format("delta").load(output_path)
  df_check.printSchema()
  df_check.count()
