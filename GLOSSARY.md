# HUST Search Engine

Vertical search engine covering public web pages and attached documents across `hust.edu.vn` and `*.hust.edu.vn`, with Vietnamese word segmentation, corpus-derived synonyms, and Pseudo-Relevance Feedback.

## Language

### Collection & Indexing

**Crawled Document**:
A public web page or attached text-bearing document (HTML, PDF, DOCX, DOC, XLSX, PPTX, TXT) collected from `hust.edu.vn` or any of its subdomains.
_Avoid_: Page, item, raw record

**Canonical URL**:
The normalized unique address of a Crawled Document after removing fragments, redundant tracking parameters, and trailing-slash duplicates.
_Avoid_: Raw URL, link, source path

**Scanned Document**:
A Crawled Document whose extracted body contains too little machine-readable text and is therefore represented only by its filename, title, and linking anchor text.
_Avoid_: Image PDF, broken file, OCR-pending file

**Crawl Cycle**:
A scheduled incremental pass that discovers new URLs, checks existing entries for content changes or removals, and synchronizes only the delta to the search index.
_Avoid_: Full re-crawl, scrape job, sync run

### Vietnamese Language Processing

**Segmented Text**:
Normalized Vietnamese text in which multi-syllable compound words are joined into single lexical units so document indexing and user querying use identical word boundaries.
_Avoid_: Cleaned text, preprocessed string

**Diacritic-Free Text**:
A parallel representation of a document's title and body with Vietnamese diacritics removed to match accent-insensitive user queries at lower priority.
_Avoid_: Unaccented copy, plain text

**Synonym Dictionary**:
A curated mapping of HUST-specific equivalent terms and abbreviations mined from the collected corpus and reviewed before activation.
_Avoid_: Thesaurus, word list

### Retrieval & Evaluation

**Synonym Expansion**:
Query-time enrichment that appends lower-weighted equivalents from the Synonym Dictionary so expansion can be toggled or updated without re-indexing.
_Avoid_: Alias indexing, query rewriting

**Pseudo-Relevance Feedback (PRF)**:
A two-pass retrieval technique that treats the top-ranked results of an initial query as relevant, extracts their most discriminative terms, and appends them at reduced weight to a second-pass query.
_Avoid_: Auto-boost, relevance loop

**Relevance Benchmark**:
A labeled set of representative HUST queries paired with document relevance judgments used to measure retrieval quality (`P@K`, `Recall@K`, `MRR`, `NDCG`).
_Avoid_: Eval dataset, test queries
