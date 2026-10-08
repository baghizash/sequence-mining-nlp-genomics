# Sequence Mining: Tweets NLP + Genome Alignment

Text and biological sequence mining: n-gram patterns in tweets, edit distance, near-duplicate detection, and a real coronavirus genome alignment (human vs. bat).

Coursework from **Data Mining in Python** (University of Michigan, More Applied Data Science with Python specialization). All notebooks run end to end with outputs.

## Data

`data/` holds:

- `tweets.txt` — 10,000 raw tweets (1.1 MB), tokenized with NLTK TweetTokenizer
- `MN908947.3_human.txt` — human-hosted coronavirus gene sequence (30 KB, NCBI)
- `MN996532.1_bat.txt` — bat-hosted coronavirus gene sequence (30 KB, NCBI)

## What was done

**Part 1 — Tweet n-grams** (`notebooks/part1-tweet-ngrams.ipynb`)
Implemented `freq_bigrams(tweets, top_n)` and `freq_skipgrams(tweets, n, k, top_n)` with NLTK TweetTokenizer + Counter to find the most frequent consecutive and non-consecutive word pairs across 10,000 tweets.

**Part 2 — Edit distance** (`notebooks/part2-edit-distance.ipynb`)
Implemented `my_edit_distance` with the Wagner–Fischer dynamic-programming recurrence (Levenshtein distance: insertions, deletions, substitutions). Reproduced both expected distance matrices exactly. This is the core of spell checkers.

**Part 3 — Near-duplicate detection** (`notebooks/part3-shingling-near-duplicates.ipynb`)
Implemented `shingling_jaccard_similarity`: represent each text as a set of overlapping n-grams (shingles) via `nltk.ngrams`, then score overlap with Jaccard similarity. Verified on the lecture examples (0.6 and 1/3). Used for plagiarism / copy-paste detection.

**Part 4 — Genome alignment** (`notebooks/part4-genome-alignment.ipynb`)
Implemented `seq_align()`: reads and cleans both coronavirus gene files (skipping `>` metadata lines; 29,132 and 29,129 nucleobases), then scores the alignment of the first 10,000 nucleobases. The direct Biopython `pairwise2.align.globalxx` call builds a 10001×10001 score matrix (~5.4 GB) and kills small kernels, so the score is computed with the mathematically identical recurrence (match +1, mismatch 0, free gaps) in O(n) memory — verified to return exactly the same score: **9572.0**, confirming the ~96% human–bat gene similarity reported in early COVID-19 literature.

## Tech

Python, NLTK (TweetTokenizer, ngrams), Biopython (pairwise2), NumPy, Jupyter.
