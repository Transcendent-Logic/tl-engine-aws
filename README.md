# Qwen3-Reranker on TL Engine for Amazon SageMaker

Sample notebook and examples for the AWS Marketplace listings of Qwen3-Reranker on TL Engine: Qwen's open rerankers (0.6B, 4B and 8B, Apache-2.0) served by Transcendent Logic's inference engine for the NVIDIA L4, as SageMaker model packages.

| Listing | Model | Instance type |
|---|---|---|
| Qwen3-Reranker-0.6B on TL Engine (AWS Marketplace link to come) | [Qwen/Qwen3-Reranker-0.6B](https://huggingface.co/Qwen/Qwen3-Reranker-0.6B) | ml.g6.xlarge |
| Qwen3-Reranker-4B on TL Engine (AWS Marketplace link to come) | [Qwen/Qwen3-Reranker-4B](https://huggingface.co/Qwen/Qwen3-Reranker-4B) | ml.g6.xlarge |
| Qwen3-Reranker-8B on TL Engine (AWS Marketplace link to come) | [Qwen/Qwen3-Reranker-8B](https://huggingface.co/Qwen/Qwen3-Reranker-8B) | ml.g6.xlarge |

All three support real-time endpoints and batch transform.

## Quick start

1. Subscribe to a listing in AWS Marketplace.
2. Open [`qwen3-reranker-sagemaker.ipynb`](qwen3-reranker-sagemaker.ipynb) in SageMaker Studio (or anywhere with Python, `boto3` and AWS credentials).
3. Set `MODEL` in the first code cell to `"0.6b"`, `"4b"` or `"8b"`, and run all cells.

The notebook creates a model with network isolation, deploys it to an ml.g6.xlarge endpoint, reranks two sample requests, runs a batch transform job, checks the results, and deletes everything it created. It takes about 20 minutes.

No environment variables or timeout changes are needed: the model package runs on SageMaker's default settings.

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

## Support

Email hello@transcendentlogic.com.

## Licence

The notebook and examples are licensed under Apache-2.0 ([LICENSE](LICENSE)). The Qwen3-Reranker models are Alibaba Cloud's, licensed under Apache-2.0 and supplied unmodified ([NOTICE](NOTICE)). TL Engine is not part of this repository.
