# Midterm 1: answers

GitHub username:  meruert-z
Session code: zebra-28
Fingerprint: a7d153dc

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used ChatGPT to help me understand the assignment instructions because some parts were difficult to understand at first. I also used it to translate my own answers into English and check whether they were written correctly. The calculations, analysis, and final answers were completed by me using the data provided in the assignment. ChatGPT helped me understand what I needed to do, but I made the final decisions and filled in the answers myself.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.5000 | 0.6167 | 0.7167 | 0.5699 |
| B | 0.3167 | 0.5833 | 0.6333 | 0.4382 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.6167 | 0.4902 | 0.7291 |
| A, k=5 | 0.7167 | 0.5923 | 0.8149 |
| B, k=3 | 0.5833 | 0.4573 | 0.6994 |
| B, k=5 | 0.6333 | 0.5068 | 0.7438 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 19612.04 |
| A, k=5 | 26028.57 |
| B, k=3 | 12274.99 |
| B, k=5 | 13800.15 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=5

I choose candidate B, k=5 because its annual cost is about $13,801, which is within the $16,700 budget. It provides better overall recall than B, k=3, with 65% for EN, 80% for RU, and 45% for KK. Although the results for Russian are good, Kazakh has the lowest recall. I think this option is a reasonable balance between cost and performance, but more testing is needed to improve Kazakh retrieval.

### 1e. Next measurement (at most 60 words)

I would test both configurations on a larger dataset with more questions in English, Russian, and Kazakh. The current results are based on only 20 questions per language, so they may not represent real performance. I would pay special attention to Kazakh retrieval because its recall is the lowest. This would help me decide whether configuration B, k=5 is the best option for production.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.8722 | KK, degraded | 0.8000 | 0.6269 | 0.9049 | $60,663 |
| L | 0.7944 | KK, degraded | 0.7000 | 0.5212 | 0.8334 | $18,000 |

Personal-data pages per year sent abroad under H: 379600

### 2b. Memo (at most 300 words)

Pipeline H has higher overall accuracy (87.22%) than Pipeline L (79.44%). However, the totals hide weaker results for degraded documents, especially in Kazakh. H's weakest cell is Kazakh degraded documents, with 24/30 correct (80.0%), while L achieves 21/30 (70.0%). The confidence intervals are wide because each group contains only 30 documents, so more testing is needed before deployment.

For personal data, three questions must be answered for both pipelines: Where is the data processed, who stores or can access it, and how long is it retained? Under H, pages are sent to an external service, so the processing location, provider access, and retention period must be confirmed. Under L, processing is on our own servers, but access permissions, server location, and retention still need verification. At 8,000 pages per day, approximately 379,600 pages containing personal data could be sent abroad annually under H, assuming the 13% rate remains constant.

I recommend L as the initial option because it keeps processing under our control and costs approximately $18,000 per year, compared with about $60,663 for H. The residual risk is lower accuracy, particularly on degraded Kazakh documents. Before launch, delete processed files after 30 days, review access logs monthly, audit access permissions every three months, and manually review cases where confidence is below 0.85. Confirm data location, access, and retention with the provider before considering H. These safeguards should be verified during a pilot.

## Part 3: Explain your own numbers

### Q1

Number: 2.74
Why (at most 40 words): Kazakh uses more tokens than English, but input and output tokens have different prices. The bill ratio is therefore different from the total token ratio.

### Q2

Number: 35.1% 
Why (at most 40 words): English produces about 789 e5 tokens, so 277 tokens are not seen by the embedding model. Russian and Kazakh stay below the 512-token limit and are not cut.

### Q3

Number: 75.6%
Why (at most 40 words): Top-p removes lower-ranked tokens once the cumulative probability reaches 0.95, then renormalizes the remaining probabilities. Lowering temperature changes the probability distribution but does not remove tokens in the same way.

### Q4

Number: +14
Why (at most 40 words): Model Y scores 52 points after penalties, while Model X scores 38. However, the public leaderboard prefers X because it has more correct answers, which can encourage guessing instead of saying “I don't know.”

### Q5

Number: 43 550
Why (at most 40 words): Each turn sends the full conversation history, so the input tokens increase every time. With 134 new tokens per turn over 25 turns, the total is 134 × (1 + 2 + ... + 25) = 43,550 tokens. Caching may reduce cost, not tokens sent.
