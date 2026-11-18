# Rerankers: Qwen3-Reranker on TL Engine

Qwen's open rerankers (0.6B, 4B and 8B, Apache-2.0) served by TL Engine on one NVIDIA L4 (ml.g6.xlarge), for real-time endpoints and batch transform.

## Notebook

[`qwen3-reranker.ipynb`](qwen3-reranker.ipynb) works for all three listings: set `MODEL` to `"0.6b"`, `"4b"` or `"8b"` in the first code cell. It creates a model with network isolation, deploys it to an ml.g6.xlarge endpoint, reranks two sample requests, runs a batch transform job, checks the results, and deletes everything it created. It takes about 20 minutes.

## Request and response

Send `application/json`:

```json
{
  "query": "What is the capital of France?",
  "documents": [
    "France is a country in Europe.",
    "Paris is the capital of France.",
    "Berlin is the capital of Germany."
  ]
}
```

The response lists the documents from most to least relevant. `index` is the document's position in the request, and `relevance_score` is between 0 and 1. `usage.total_tokens` counts the tokens processed:

```json
{
  "id": "score-a2045f42aa711d4e",
  "model": "reranker",
  "usage": {"prompt_tokens": 255, "total_tokens": 255},
  "results": [
    {"index": 1, "document": {"text": "Paris is the capital of France.", "multi_modal": null}, "relevance_score": 0.9972},
    {"index": 0, "document": {"text": "France is a country in Europe.", "multi_modal": null}, "relevance_score": 0.0032},
    {"index": 2, "document": {"text": "Berlin is the capital of Germany.", "multi_modal": null}, "relevance_score": 0.0005}
  ]
}
```

Scores shown are the 0.6B's, rounded; the full responses of each size, from the model packages on 2026-11-18, are in [`samples/`](samples/).

With the AWS CLI:

```bash
aws sagemaker-runtime invoke-endpoint --endpoint-name <your endpoint> \
  --content-type application/json --cli-binary-format raw-in-base64-out \
  --body '{"query": "What is the capital of France?", "documents": ["Berlin is the capital of Germany.", "Paris is the capital of France."]}' \
  out.json
```

## Limits

- Up to 33,000 tokens for a query and one document.
- Up to 25 MB per real-time request and 100 MB per batch request.
- Batch transform takes one request per input file and writes the response to `<input file name>.out`.

