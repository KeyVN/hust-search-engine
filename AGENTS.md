# HUST Search Engine

Vertical search engine for `hust.edu.vn` and `*.hust.edu.vn` built with Python (`crawler-nlp`), Apache Tika, PostgreSQL, and Java 21 + Spring Boot + Apache Lucene 10.1.0 (`search-service`).

## Repository Layout

- `crawler-nlp/` — Python service (Scrapy, Playwright, PyVi, FastAPI `/tokenize`, Gensim synonym pipeline, APScheduler)
- `search-service/` — Java 21 + Maven + Spring Boot + Apache Lucene 10.1.0 + static Web UI
- `evaluation/` — IR evaluation scripts and labeled query/relevance benchmarks (`P@K`, `MRR`, `NDCG`)
- `docs/adr/` — Architecture Decision Records
- `GLOSSARY.md` — Canonical domain vocabulary

## Agent skills

### Issue tracker

Local markdown files under `.scratch/<feature-slug>/`. See `docs/agents/issue-tracker.md`.

### Triage labels

Default five canonical triage roles (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`). See `docs/agents/triage-labels.md`.

### Domain docs

Single-context repo (`GLOSSARY.md` and `docs/adr/` at the repo root). See `docs/agents/domain.md`.
