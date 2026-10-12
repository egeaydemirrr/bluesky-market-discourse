# Financial-Market Discourse on Bluesky

How do people talk about markets on Bluesky, who shapes the conversation, and does the mood follow prices? A network + NLP study of one month of English-language posts about stocks, the Fed and crypto (13 Aug – 12 Sep 2026). Final project for *Web and Social Media Search and Analysis*, BSc Artificial Intelligence (University of Milan · Milano-Bicocca · Pavia).

**Read the [report](report.pdf)** or the [slides](slides.pdf).

## What was done

| Step | Notebook | Methods |
|---|---|---|
| Data collection | `01_collect` | AT Protocol API (`searchPosts`, `getPostThread`), 7 queries, exhaustive time window, rate-limit handling → **34,533 posts + 28,556 replies** |
| Reply network | `02_network` | directed reply graph (**15,973 nodes, 18,980 edges**), bot/automated-account rules, degree distribution, centralities |
| Communities | `03_communities` | Louvain, greedy modularity (CNM), label propagation, FluidC; stability across seeds |
| Content | `04_content` | spaCy preprocessing and NER, sentiment with VADER, AFINN and RoBERTa, NRC emotions |
| Results | `05_results` | communities × sentiment, timeline against market events, echo-chamber test, correlation with Bitcoin and S&P 500 (yfinance) |

## Key findings

- **Strongly clustered conversation.** Louvain finds 67 communities with modularity Q = 0.86; 87.5% of reply edges stay inside a community.
- **Lexicon sentiment is weak on social text.** Validated on TweetEval, RoBERTa reaches 72% accuracy vs. 52–55% for VADER and AFINN.
- **Crypto talk tracks the market.** Daily Bitcoin post volume correlates with same-day trading volume (r = 0.70, p < 0.001), and sentiment moves with returns (r = 0.43).
- **Associations, not predictions.** Next-day relationships are weaker and the S&P sample has only 17 trading days, so the results do not show that posts predict prices.

## Data

The raw posts and per-user tables are **not included**, to respect the privacy of Bluesky users. The repo keeps the aggregate result tables in `data/processed/` and every figure in `figures/`.

To rebuild the dataset, run `01_collect` with your own Bluesky app password, saved outside the repo as `../secrets/bluesky_credentials.json`:

```json
{"HANDLE": "your.handle.bsky.social", "APP_PASSWORD": "xxxx-xxxx-xxxx-xxxx"}
```

Then run notebooks 02 → 05 in order. Re-collecting today gives a slightly different set (deleted posts, new replies).

## Setup

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

Random seeds are fixed (`SEED = 42`). RoBERTa (`cardiffnlp/twitter-roberta-base-sentiment-latest`) downloads from Hugging Face on first use.

## License

Code: MIT
