# PeopleProfiles
Official Repository for the PeopleProfiles dataset, introduced in the following paper:

> *How Grounded is Wikipedia? A Study on Structured Evidential Support.* William Walden, Kathryn Ricci, Miriam Wanner, Zhengping Jiang, Chandler May, Rongkun Zhou, and Benjamin Van Durme. 2025.

## Contents

- `data/`:
  - The `data` for the three different tasks, described in greater detail below (in [Downloading the Dataset](#downloading-the-dataset))
  - The `qrels` files for each task (see `results/` below). `qrels` files with a number (1, 2, 3) in the file name represent qrels for only those examples/claims that have exactly that many evidence sentences annotated.
- `results/`: All runfiles and scores for test set results on the **LEAD** (`lead/`), **BODY** (`body/`), and **ENTITY** (`entity/`) evidence retrieval tasks reported in the paper. Scores are computed using [trec_eval](https://github.com/usnistgov/trec_eval) (with the `-m 'all_trec'` option set). Results with BM25, Stella-en-1.5B-v5, and ColBERT-v2 are all first-stage retrieval results; results with Rank1 are reranking results on the BM25 outputs (top 10 for **LEAD** and **BODY**; top 100 for **ENTITY**).

## Data

### Overview

The PeopleProfiles dataset has three subsets, each corresponding to one of the three tasks described above and in the paper:

- **LEAD**: claims from Wikipedia *lead* sections paired with evidence sentences from the *body* of the same Wikipedia article, with scalar annotations on the claim indicating the degree of support for that claim given the evidence, ranging from -1 (full refutation) to +1 (full support). Claims with support scores of 0 have empty evidence sets; all other claims have non-empty evidence sets.
- **BODY**: claims from Wikipedia *body* sections paired with evidence from a single *source* document cited by that sentence, along with scalar support annotations.

## Data Format 

### LEAD

Each item in the **LEAD** data has a number of fields. For most use cases, including all the experiments presented in the paper, the following fields are the most important:

- `instance-id` (`str`): A unique identifier for the example
- `report-title` (`str`): The name of the Wikipedia article this example came from
- `subclaim-decontext`: The *decontextualized* claim for this example (used as the query for the **LEAD** retrieval task)
- `subclaim` (`str`): The *non-decontextualized* claim for this example
- `subclaim-idx` (`int`): The index of the `subclaim`/`subclaim-decontext` within the list of *all* claims decomposed from the sentence (NOTE: these other claims are not included in the example; each claim is an example unto itself).
- `score` (`float`): The support score for the `subclaim`/`subclaim-decontext` given the evidence (`snippet-sents`)
- `source-text` (`str`): The full text of the body of the Wikipedia article (excluding the lead section), given as a list of sentences
- `snippet-sent-idxs` (`List[int]`): The indices of the evidence sentences for the `subclaim`/`subclaim-decontext` within the `source-text`
- `snippet-sents` (`List[str]`): The texts of the above evidence sentences
- `claim-sentence` (`str`): The sentence from which the claim for this example was decomposed 

The following additional fields may also be helpful &dash; notably if you are doing work with MegaWika 2.0 &mdash; the dataset on which PeopleProfiles is based. 

- `report-id` (`str`): A unique identifier for the Wikipedia lead section this example came from (NOTE: in PeopleProfiles, the `report-title` is also a unique identifier for these lead sections)
- `paragraph-text` (`List[str]`): The paragraph within the Wikipedia article that this lead claim came from, given as a list of sentences
- `paragraph-elt-idx` (`int`): The index of the above paragraph within the list of all paragraphs for this section, as given in MegaWika 2.0.
- `source-elt-idxs` (`List[List[int]]`): Mappings from each sentence in the Wikipedia body to the index of the section within the article (a MegaWika 2.0 "element") it belongs to.
- `sentence-idx` (`int`): The index of the sentence from which the claim was decomposed within the above paragraph
- `is-citation-sent` (`bool`): Whether the lead sentence associated with this example has a citation attached to it in the original text
- `source-elt-idxs`: The element in

### BODY

The **BODY** data has many of the same fields as the **LEAD** data, although some of them have different interpretations. The most important fields are the same as the ones for the **LEAD** data:

TODO