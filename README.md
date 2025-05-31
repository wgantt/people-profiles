# PeopleProfiles
Official Repository for the PeopleProfiles dataset, introduced in the following paper:

> *How Grounded is Wikipedia? A Study on Structured Evidential Support.* William Walden, Kathryn Ricci, Miriam Wanner, Zhengping Jiang, Chandler May, Rongkun Zhou, and Benjamin Van Durme. 2025.

## Contents

- `data/`: Currently just contains the `qrels` files for each task (see `results/` below). `qrels` files with a number (1, 2, 3) in the file name represent qrels for only those examples/claims that have exactly that many evidence sentences annotated.
- `results/`: All runfiles and scores for test set results on the **LEAD** (`lead/`), **BODY** (`body/`), and **ENTITY** (`entity/`) evidence retrieval tasks reported in the paper. Scores are computed using [trec_eval](https://github.com/usnistgov/trec_eval) (with the `-m 'all_trec'` option set). Results with BM25, Stella-en-1.5B-v5, and ColBERT-v2 are all first-stage retrieval results; results with Rank1 are reranking results on the BM25 outputs (top 10 for **LEAD** and **BODY**; top 100 for **ENTITY**).

## Downloading the Dataset

TODO