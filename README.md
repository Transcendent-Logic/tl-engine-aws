# TL Engine on AWS

Sample notebooks and examples for Transcendent Logic's AWS Marketplace listings: open models served by TL Engine, our inference engine for the NVIDIA L4, as Amazon SageMaker model packages. Each runs in your own AWS account as a real-time endpoint or a batch transform job, with network isolation.

## Listings

| Listing | Model | Instance type | Examples |
|---|---|---|---|
| Qwen3-Reranker-0.6B on TL Engine (AWS Marketplace link to come) | [Qwen/Qwen3-Reranker-0.6B](https://huggingface.co/Qwen/Qwen3-Reranker-0.6B) | ml.g6.xlarge | [rerankers/](rerankers/) |
| Qwen3-Reranker-4B on TL Engine (AWS Marketplace link to come) | [Qwen/Qwen3-Reranker-4B](https://huggingface.co/Qwen/Qwen3-Reranker-4B) | ml.g6.xlarge | [rerankers/](rerankers/) |
| Qwen3-Reranker-8B on TL Engine (AWS Marketplace link to come) | [Qwen/Qwen3-Reranker-8B](https://huggingface.co/Qwen/Qwen3-Reranker-8B) | ml.g6.xlarge | [rerankers/](rerankers/) |

## Quick start

1. Subscribe to a listing in AWS Marketplace.
2. Open its notebook in SageMaker Studio, or anywhere with Python, `boto3` and AWS credentials.
3. Set the size in the first code cell and run all cells.

The notebooks need no data of your own, check their results, and delete everything they create. No environment variables or timeout changes are needed: the model packages run on SageMaker's default settings.

## Support

Email hello@transcendentlogic.com.

## Licence

The notebooks and examples are licensed under Apache-2.0 ([LICENSE](LICENSE)). The models are their authors', under their own licences; the listings carry Qwen's weights converted to TL Engine's own model file format ([NOTICE](NOTICE)). TL Engine is not part of this repository.
