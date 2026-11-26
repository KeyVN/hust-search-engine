---
status: accepted
---

# Hybrid 3-Service Architecture with Apache Lucene 10.1.0 and Python PyVi

To satisfy the requirement of using Apache Lucene 10.1.0 directly (which requires Java 21+) while leveraging Python's mature crawling and Vietnamese NLP ecosystem, we split the system into three Dockerized services backed by PostgreSQL: `crawler-nlp` (Python: Scrapy + selective Playwright + PyVi + Gensim + FastAPI `/tokenize`), `tika` (official Apache Tika Server), and `search-service` (Java 21 + Maven + Spring Boot + Lucene 10.1.0 + static Web UI). Documents flow from `crawler-nlp` to `search-service` via internal REST batch endpoints, and query-time Vietnamese segmentation calls `POST /tokenize` (with LRU caching) so index-time and query-time tokens stay identical.

## Considered Options

- **All-Java stack (Lucene + Java crawler + embedded Tika/VnCoreNLP)**: Avoids cross-service HTTP calls, but loses Scrapy/Playwright ergonomics for mixed static/SPA HUST sites and Gensim Word2Vec/FastText tooling for corpus-driven synonym mining.
- **Elasticsearch / OpenSearch instead of embedded Lucene 10.1.0**: Simplifies clustering and REST indexing, but abstracts away Lucene 10.1.0 internals needed for custom `SynonymGraphFilter` query-time toggling and explicit 2-pass TermVector-based PRF experiments.
- **Index-time synonym expansion**: Faster at query time, but forces a full re-index whenever `synonyms.txt` changes and prevents side-by-side A/B toggling on the UI and Relevance Benchmark.

## Consequences

- `search-service` depends on `crawler-nlp`'s `/tokenize` endpoint at query time; an in-memory LRU cache and graceful fallback to whitespace tokenization are required to keep latency low and resilient.
- Because `Segmented Text` joins compound words with underscores (`sinh_viên`), `search-service` must store raw fields (`title_raw`, `content_raw`) alongside indexed fields so UI snippets and highlights never expose underscores to users.
- Query-time `SynonymGraphFilter` plus `POST /internal/synonyms/reload` enables instant hot-reload and clean Baseline vs. +Synonym vs. +PRF comparisons without re-indexing.
