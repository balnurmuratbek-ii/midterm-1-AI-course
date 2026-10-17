# Midterm 1: answers

GitHub username: balnurmuratbek
Session code: zebra-28
Fingerprint: df4f2a9f

## AI-use declaration

I used Gemini solely to compute, format, and structure the responses according to the rubric requirements. All calculations, Wilson score confidence intervals, and budget optimizations were verified for precision.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
| :--- | :---: | :---: | :---: | :---: |
| A | 0.433 | 0.650 | 0.733 | 0.548 |
| B | 0.350 | 0.533 | 0.617 | 0.442 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
| :--- | :---: | :---: | :---: |
| A, k=3 | 0.650 | 0.524 | 0.758 |
| A, k=5 | 0.733 | 0.609 | 0.829 |
| B, k=3 | 0.533 | 0.408 | 0.654 |
| B, k=5 | 0.617 | 0.490 | 0.728 |

### 1c. Annual model cost in USD

| candidate | annual cost |
| :--- | :---: |
| A, k=3 | 13678.33 |
| A, k=5 | 18918.78 |
| B, k=3 | 8705.98 |
| B, k=5 | 10631.53 |

### 1d. Decision (at most 150 words)

Chosen candidate: A, k=5

I recommend Candidate (A, k=5) with an annual model cost of 18918.78 USD, which strictly respects the 20200 USD budget constraint. Config A clearly outperforms Config B on all retrieval metrics. Although its 95% Wilson interval [0.609, 0.829] overlaps with (A, k=3) [0.524, 0.758], candidate (A, k=5) yields the highest absolute recall of 73.3%. Breakdowns by language show: EN = 80.0%, RU = 75.0%, KK = 65.0%. Kazakh performance is lower due to sub-optimal tokenization, but (A, k=5) maximizes overall retrieval within budget.

### 1e. Next measurement (at most 60 words)

Evaluate end-to-end downstream answer accuracy on 100 queries using human judges comparing (A, k=3) vs (A, k=5) to confirm if the 8.3% recall gain improves final generated response quality.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
| :--- | :---: | :--- | :---: | :---: | :---: | :---: |
| H | 0.894 | Kazakh, Degraded | 0.833 | 0.664 | 0.927 | 10596.86 |
| L | 0.772 | Kazakh, Degraded | 0.533 | 0.361 | 0.698 | 9000.00 |

Personal-data pages per year sent abroad under H: 344925

### 2b. Memo (at most 300 words)

I recommend deploying Pipeline L (local open model in Kazakhstan) over Pipeline H, despite H's higher overall accuracy (89.4% vs 77.2%).

Pipeline L performs worst on Kazakh/Degraded scans at 53.3% accuracy [95% CI: 0.361, 0.698], compared to 83.3% [0.664, 0.927] for Pipeline H. However, Kazakhstan's Personal Data Law prohibits sending individual names and ИИН abroad. Under Pipeline H, 344,925 personal data pages annually leave the country to foreign API servers, exposing the bank to severe legal liability. Pipeline L guarantees 0 pages leave Kazakhstan, completely mitigating compliance risk at a lower annual cost ($9000 vs $10596.86). Automated confidence routing will send low-confidence extractions to human operators to manage accuracy risks.

## Part 3: Explain your own numbers

### Q1

Number: 2.87  
Why (at most 40 words): Output tokens cost 6x input tokens ($18 vs $3 per 1M). Kazakh requires more input and output tokens due to tokenization fragmentation, raising the bill ratio from the raw token ratio.

### Q2

Number: 13.5%  
Why (at most 40 words): English expands by a 263/256 factor to 592 tokens, exceeding the 512-token limit. Trailing text is truncated at token 512, completely losing those embeddings.

### Q3

Number: 51.3%  
Why (at most 40 words): Temperature scaling divides logits before Softmax. T=1.5 flattens probability distributions, lowering top-token confidence without changing token rankings or candidate sets.

### Q4

Number: 26.5%  
Why (at most 40 words): Arithmetic and sequence queries test reasoning rather than factual retrieval. Lower bounds apply to synthetic profiles/public figures; system prompts cannot recover single-exposure unlearned facts.

### Q5

Number: 7644  
Why (at most 40 words): The first message is re-sent across 12 turns without caching. Prompt caching bills cached prefixes at a reduced rate, preventing repeated full-cost input processing.
