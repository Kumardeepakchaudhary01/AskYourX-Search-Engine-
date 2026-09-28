<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=800&size=45&pause=1000&color=6C63FF&center=true&vCenter=true&width=700&lines=AskYourX+%F0%9F%94%8D;Distributed+Search+Engine;Built+from+Scratch+in+C%2B%2B17" alt="AskYourX" />

<br/>

<p>
  <img src="https://img.shields.io/badge/C%2B%2B-17-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white"/>
  <img src="https://img.shields.io/badge/CMake-064F8C?style=for-the-badge&logo=cmake&logoColor=white"/>
  <img src="https://img.shields.io/badge/gRPC-4285F4?style=for-the-badge&logo=google&logoColor=white"/>
  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB"/>
  <img src="https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white"/>
  <img src="https://img.shields.io/badge/GoogleTest-4285F4?style=for-the-badge&logo=google&logoColor=white"/>
</p>

<p>
  <img src="https://img.shields.io/badge/status-under%20active%20development-orange?style=flat-square&logo=git"/>
  <img src="https://img.shields.io/badge/license-TBD-lightgrey?style=flat-square"/>
  <img src="https://img.shields.io/badge/PRs-welcome-blueviolet?style=flat-square"/>
  <img src="https://img.shields.io/badge/platform-Linux%20%7C%20macOS%20%7C%20Windows-informational?style=flat-square"/>
</p>

<br/>

<blockquote>
<b>AskYourX</b> is an engineering project exploring the core ideas behind modern search infrastructure:<br/>
document ingestion · text analysis · inverted indexing · ranked retrieval · persistent indexes · distributed query execution.
</blockquote>

> ⚠️ **Project status:** Under active development. Features marked as *Planned* are not yet implemented. Do not use this as a production search service until its correctness, security, reliability, and operational behavior have been validated.

<br/>

</div>

---

## 📋 Table of Contents

<details>
<summary><b>Click to expand</b></summary>

- [🔭 Overview](#-overview)
- [🎯 Goals](#-goals)
- [🗺️ Features and Roadmap](#️-features-and-roadmap)
- [⚙️ How Search Works](#️-how-search-works)
- [🏗️ Architecture](#️-architecture)
- [🛠️ Technology Stack](#️-technology-stack)
- [📁 Repository Structure](#-repository-structure)
- [🚀 Getting Started](#-getting-started)
- [🔨 Build and Test](#-build-and-test)
- [💡 Usage](#-usage)
- [🌐 API](#-api)
- [📊 Data Model](#-data-model)
- [🏆 Ranking and Retrieval](#-ranking-and-retrieval)
- [🌍 Distributed Design](#-distributed-design)
- [🛡️ Reliability and Consistency](#️-reliability-and-consistency)
- [📈 Performance and Benchmarking](#-performance-and-benchmarking)
- [🔒 Security and Privacy](#-security-and-privacy)
- [👩‍💻 Development Workflow](#-development-workflow)
- [📐 Design Decisions](#-design-decisions)
- [⚠️ Limitations](#️-limitations)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

</details>

---

## 🔭 Overview

Search systems need to find relevant information **without scanning every document for every query**. AskYourX is intended to demonstrate how that can be achieved and how the same search workload can be distributed across multiple machines.

The project is being developed in **six progressive stages**:

```
Stage 1 ──► Tested, in-memory single-node index and query engine
Stage 2 ──► Relevance ranking and richer query behavior
Stage 3 ──► Persistent indexes with updates, deletion, and recovery
Stage 4 ──► Sharded index with distributed query coordination
Stage 5 ──► Replication, health checks, metrics, and fault-handling
Stage 6 ──► Usable API, dashboard, evaluation corpus, and benchmarks
```

> The repository is both a **working software project** and a place to document the reasoning behind its system design. Architecture diagrams, benchmarks, and design decisions should be updated as the implementation evolves.

---

## 🎯 Goals

| Goal | Description |
|------|-------------|
| ✅ **Correctness first** | Define behavior precisely and test edge cases before optimizing |
| 🔍 **Understandable internals** | Implement key search structures and algorithms rather than hiding behavior behind managed services |
| 📏 **Measurable performance** | Report benchmark methodology and measured results, not unsupported performance claims |
| 📦 **Incremental scalability** | Keep the first version simple while making module boundaries suitable for later distribution |
| 👁️ **Operational clarity** | Make indexing state, query behavior, errors, and node health observable |
| 🔁 **Reproducibility** | Provide documented build, test, and benchmark steps |

### 🚫 Non-goals for the Initial Milestone

- Replacing a mature search product for production workloads
- Crawling the entire public web
- Providing a hosted search service or multi-tenant SaaS
- Claiming exactly-once processing or linearizable behavior before those guarantees are designed and tested
- Supporting every language, file format, or advanced search feature from day one

---

## 🗺️ Features and Roadmap

> The following is a **target roadmap**, not a claim that every item is already implemented. Update the status column as work lands.

| Area | Planned Capability | Status |
|------|--------------------|--------|
| 📥 Ingestion | Add documents through a defined input format | `Planned` |
| 📝 Text Analysis | Tokenization, normalization, configurable analyzers | `Planned` |
| 🗂️ Indexing | Inverted index with posting lists | `Planned` |
| 🔎 Retrieval | Term lookup and Boolean AND/OR queries | `Planned` |
| 🏆 Ranking | TF-IDF baseline and BM25 scoring | `Planned` |
| 💬 Query Features | Phrase queries, field filters, snippets | `Planned` |
| 💾 Storage | Persistent index segments and metadata | `Planned` |
| ✏️ Mutations | Update and delete documents | `Planned` |
| 🔄 Recovery | Reopen index after restart and validate stored data | `Planned` |
| 🌐 Distribution | Shard routing and parallel fan-out queries | `Planned` |
| 🛡️ Resilience | Node health, replica strategy, recovery workflow | `Planned` |
| 🌐 API | HTTP endpoints and request validation | `Planned` |
| 🖥️ UI | Search interface and index/cluster status | `Planned` |
| 🧪 Quality | Unit, integration, and fault-injection tests | `Planned` |
| 📊 Observability | Metrics, structured logs, tracing where appropriate | `Planned` |

---

## ⚙️ How Search Works

### Full Pipeline

```
╔══════════════════════════════════════════════════════════════╗
║                         INDEXING                            ║
╚══════════════════════════════════════════════════════════════╝

  Documents ──► Parse ──► Analyze text ──► Build postings ──► Store index
                                                              │
                                                              ▼
╔══════════════════════════════════════════════════════════════╗
║                         QUERYING                            ║
╚══════════════════════════════════════════════════════════════╝

  User query ──► Parse/analyze ──► Retrieve candidates ──► Score
                                                              │
                                                              ▼
                                                    Sort top results
                                                              │
                                                              ▼
                                                     Return response
```

### 1. 📥 Ingestion

Documents enter through supported input adapters. An adapter validates the input, extracts searchable fields, and assigns or checks a stable document identifier. The initial version favors simple, deterministic formats such as **JSON Lines** or **plain text**.

### 2. 🔤 Text Analysis

The analyzer converts text into terms used by the index. The first analyzer may perform basic **tokenization and normalization**. More advanced analyzers can add:
- Unicode-aware tokenization
- Stop-word handling
- Stemming
- Language-specific rules
- Per-field analysis

> ⚠️ Analyzer configuration is part of **index compatibility**: changing analysis rules can require a reindex.

### 3. 📋 Inverted Indexing

For each term, the index stores a **posting list** identifying documents that contain it. Posting entries can be extended with term frequency and token positions for ranking and phrase search.

### 4. 🔎 Query Execution

The query engine analyzes the user query, finds matching postings, combines them according to query semantics, scores candidates, and returns the highest-ranked results. Query parsing and execution are kept separate so new operators do not complicate the storage layer.

### 5. 🌐 Distributed Execution

In a distributed configuration, the coordinator sends a query to the shards that may contain matching documents. Shards search locally and return their local top results; the coordinator merges them into a global result set. Timeouts, partial results, and shard failures must be represented explicitly in the response.

---

## 🏗️ Architecture

AskYourX uses **logical modules** so core search behavior can be tested independently from networking and the user interface.

```
                     ┌──────────────────────────┐
                     │    Client / Search UI    │
                     └──────────┬───────────────┘
                                │ HTTP
                     ┌──────────▼───────────────┐
                     │   API / Query Gateway    │
                     └──────────┬───────────────┘
                                │
                     ┌──────────▼───────────────┐
                     │    Query Coordinator     │
                     │   parse · route · merge  │
                     └────────┬─────────┬───────┘
                              │         │
                    ┌─────────▼───┐ ┌───▼─────────┐
                    │   Shard A   │ │   Shard B   │  ... future shards
                    │   Search    │ │   Search    │
                    └─────────┬───┘ └───┬─────────┘
                              │         │
                    ┌─────────▼───┐ ┌───▼─────────┐
                    │  Index +    │ │  Index +    │
                    │  Storage    │ │  Storage    │
                    └─────────────┘ └─────────────┘

Ingestion ──► Validation ──► Analyzer ──► Index writer ──► Shard storage
```

### Module Responsibilities

| Module | Responsibility |
|--------|---------------|
| `common` | Shared types, error model, identifiers, and utilities |
| `ingestion` | Input adapters, validation, and document lifecycle |
| `text` | Tokenization, normalization, and analyzer configuration |
| `index` | Term dictionary, postings, index writer, and reader |
| `storage` | Files, segments, metadata, checksums, and recovery |
| `query` | Query parsing, planning, execution, and result structures |
| `ranking` | Scoring functions and ranking configuration |
| `distributed` | Shard metadata, routing, RPC, fan-out, and result merging |
| `api` | External endpoints, serialization, validation, and error mapping |
| `web` | Browser-based search and administrative UI |

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| ⚙️ **Search Core** | C++17 | Indexing, retrieval, ranking, and storage internals |
| 🏗️ **Build** | CMake | Portable project configuration and builds |
| 🔌 **RPC** | gRPC / Protocol Buffers | Planned node-to-node communication |
| 🌐 **HTTP API** | C++ HTTP library *(to be selected)* | Planned client-facing interface |
| 🖥️ **UI** | React + TypeScript | Planned search and status dashboard |
| 🗄️ **Metadata** | SQLite *(initially; alternatives evaluated later)* | Local metadata during early development |
| 🧪 **Tests** | GoogleTest | Unit and integration tests |
| 📊 **Benchmarks** | Google Benchmark or a dedicated harness | Reproducible performance measurement |
| 📡 **Metrics** | Prometheus-compatible | Planned operational visibility |
| 📈 **Visualization** | Grafana | Planned dashboards |

> The actual dependency list should be kept in `CMakeLists.txt` and the build documentation. Avoid introducing a dependency solely for a feature that can be implemented cleanly within the project.

---

## 📁 Repository Structure

```
AskYourX/
├── 📄 CMakeLists.txt
├── 📄 README.md
├── 📄 LICENSE
├── 📄 .gitignore
│
├── 📂 docs/
│   ├── architecture.md
│   ├── design-decisions.md
│   ├── query-language.md
│   ├── storage-format.md
│   └── benchmarks.md
│
├── 📂 include/
│   └── askyourx/
│       ├── common/      ├── ingestion/   ├── text/
│       ├── index/       ├── storage/     ├── query/
│       ├── ranking/     ├── distributed/ └── api/
│
├── 📂 src/
│   ├── common/          ├── ingestion/   ├── text/
│   ├── index/           ├── storage/     ├── query/
│   ├── ranking/         ├── distributed/ └── api/
│
├── 📂 apps/
│   ├── cli/
│   └── server/
│
├── 📂 tests/
│   ├── unit/
│   ├── integration/
│   └── fixtures/
│
├── 📂 benchmarks/
├── 📂 examples/
├── 📂 scripts/
├── 📂 data/             # local sample data — do not commit private/copyrighted corpora
└── 📂 web/              # planned React + TypeScript UI
```

> The repository may differ while the project is being built. Keep public headers in `include/` and implementation files in `src/` once the codebase reaches that structure.

---

## 🚀 Getting Started

### Prerequisites

| Requirement | Minimum Version |
|-------------|----------------|
| C++ Compiler (GCC / Clang / MSVC) | C++17 support |
| CMake | 3.20+ |
| Git | Latest stable |
| GoogleTest | As declared in CMakeLists.txt |

> The exact supported compiler and dependency versions should be verified in CI and documented here when the initial build is committed.

### Clone

```bash
git clone https://github.com/<your-github-username>/AskYourX.git
cd AskYourX
```

> Replace `<your-github-username>` with the account that owns the repository.

---

## 🔨 Build and Test

### Configure and Build

```bash
# Release build
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --parallel

# Debug build
cmake -S . -B build-debug -DCMAKE_BUILD_TYPE=Debug
cmake --build build-debug --parallel
```

### Run Tests

```bash
ctest --test-dir build --output-on-failure
```

### Recommended Test Categories

| Category | What It Covers |
|----------|---------------|
| 🔬 **Unit tests** | Tokenization, normalization, posting-list operations, Boolean evaluation, scoring, serialization |
| 🔗 **Integration tests** | Ingest documents, build/open an index, execute queries, verify returned IDs and ordering |
| 🔄 **Recovery tests** | Simulate interruption or corrupted files and verify safe startup behavior |
| 🌐 **Distributed tests** | Simulate unavailable shards, delayed responses, retries, and partial results |
| 🐛 **Regression tests** | Preserve a fixture and expected output for every fixed correctness bug |

---

## 💡 Usage

> The CLI and API contracts are not finalized yet. The examples below illustrate the **intended interaction**; they are not guaranteed to match a released executable.

### Intended CLI Workflow

```bash
# Add documents to an index
askyourx index --path ./examples/documents --index ./data/demo-index

# Search the index
askyourx search --index ./data/demo-index --query "distributed systems"

# Inspect index information
askyourx index-info --index ./data/demo-index
```

### Example Documents (JSON Lines)

```json
{"id":"doc-001","title":"Distributed Systems","body":"An introduction to distributed computing and coordination."}
{"id":"doc-002","title":"Search Infrastructure","body":"Inverted indexes enable efficient text retrieval."}
```

> Stable document IDs allow the engine to distinguish **inserts** from **updates** and **deletions**.

---

## 🌐 API

The public API will be versioned and documented when implemented.

### Planned REST Endpoints

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/documents` | Add or update a document |
| `DELETE` | `/v1/documents/{id}` | Delete a document |
| `POST` | `/v1/search` | Execute a search query |
| `GET` | `/v1/health` | Check service health |
| `GET` | `/v1/indexes/{id}` | Inspect index metadata |

### Example Search Request *(proposed)*

```json
{
  "query": "distributed systems",
  "limit": 10,
  "offset": 0
}
```

### Example Response *(proposed)*

```json
{
  "query": "distributed systems",
  "total_hits": 2,
  "results": [
    {
      "id": "doc-001",
      "title": "Distributed Systems",
      "score": 1.42,
      "snippet": "An introduction to distributed computing..."
    }
  ],
  "partial": false
}
```

> ⚠️ Response fields, score interpretation, pagination semantics, and error formats must be finalized and tested before being treated as a stable API. Scores are relative to the configured ranking model and should not be presented as probabilities.

---

## 📊 Data Model

| Concept | Description |
|---------|-------------|
| **Document** | Stable ID, searchable fields, and optional metadata |
| **Term** | Normalized token represented in the term dictionary |
| **Posting** | Document reference and term-specific data (frequency, positions) |
| **Index** | Collection of term dictionaries, posting lists, and document metadata |
| **Segment** | Immutable on-disk unit of indexed data, once persistence is introduced |
| **Shard** | Independently searchable partition of the document collection |
| **Query** | Parsed representation of user search intent |
| **Search Result** | Document reference, score, and optional snippet/highlight data |

### Indexing Considerations

- Document IDs must be **stable and validated**
- Analyzer settings should be **recorded with the index**
- Updates and deletes need clear semantics; immutable segments can use tombstones until compaction
- Posting lists should be **ordered by document ID** to enable efficient intersections and compression
- Index formats should include **versioning and integrity checks** before being relied upon for recovery

---

## 🏆 Ranking and Retrieval

### Boolean Retrieval

For an **AND** query, intersect the posting lists for all terms. For an **OR** query, compute their union while avoiding duplicate document IDs. Use sorted posting lists to support efficient merge-based operations.

### TF-IDF Baseline

TF-IDF provides a useful first scoring model for learning and testing term weighting. The exact term-frequency and inverse-document-frequency formulas should be documented alongside the implementation, including smoothing choices.

### BM25 *(Primary Ranking Model)*

BM25 is the planned primary lexical ranking model. Its score depends on:
- Term frequency
- Inverse document frequency
- Document length
- Corpus-average document length

Parameter choices (commonly `k1` and `b`) should be **configurable and evaluated** against a relevance test set.

### Relevance Evaluation Metrics

| Metric | Description |
|--------|-------------|
| **Precision@K** | Proportion of the first K results judged relevant |
| **Recall@K** | Proportion of known relevant documents retrieved in the first K |
| **MRR** | Reciprocal rank of the first relevant result, averaged over queries |
| **NDCG@K** | Ranking quality when relevance labels have multiple grades |

> Record the dataset, judgments, preprocessing, and metric implementation so evaluations can be **reproduced**.

---

## 🌍 Distributed Design

Distribution is a later milestone, but its **boundaries should be considered from the start**.

### Sharding

| Strategy | Pros | Cons |
|----------|------|------|
| **Hash-based routing** | Evenly spreads documents | No ordered access |
| **Range-based routing** | Supports ordered access patterns | May create hotspots |

### Query Fan-out and Merge

The coordinator sends a query to the relevant shards **concurrently**, collects shard-local top-K results, and merges them using the same ranking semantics. The system must define how it handles:
- Timeouts
- Duplicate hits
- Missing shards
- Inconsistent index versions

### Replication

The design will specify:
- Primary/replica responsibilities
- Acknowledgement behavior
- Replica freshness
- Recovery before implementation

### Cluster Membership

Node discovery and shard assignment need a source of truth. The project should document:
- How membership changes are published
- How routing metadata is versioned
- What happens during network partitions

---

## 🛡️ Reliability and Consistency

Reliability features should be implemented with **explicit guarantees and tests** rather than broad claims.

Planned areas include:

- ✅ Atomic publication of completed index segments
- ✅ Validation and checksums for persisted files
- ✅ Safe recovery after process interruption
- ✅ Bounded retries with backoff for transient operations
- ✅ Explicit request deadlines and cancellation
- ✅ Idempotent document writes where feasible
- ✅ Defined behavior for partial search results
- ✅ Health checks that distinguish process health from shard/index readiness
- ✅ Fault-injection tests for node loss and network delay

> ⚠️ The project will document its actual durability and consistency guarantees as they become implemented. Until then, avoid describing the engine as *strongly consistent*, *highly available*, or *fault tolerant* in a production sense.

---

## 📈 Performance and Benchmarking

Performance is a design concern, but **measurements must be reproducible**.

### Metrics to Track

<table>
<tr>
<th>🗂️ Indexing</th>
<th>🔎 Search</th>
<th>🌐 Distributed</th>
</tr>
<tr>
<td>

- Documents indexed/sec
- Input bytes processed/sec
- Index size and bytes written
- Peak resident memory
- Time to commit and reopen

</td>
<td>

- Throughput (queries/sec)
- Latency (p50 · p95 · p99)
- Performance by query type
- Candidates visited and postings decoded

</td>
<td>

- Coordinator overhead
- Per-shard latency and fan-out time
- Network bytes per query
- Behavior under slow/unavailable shards

</td>
</tr>
</table>

### Benchmark Methodology

For every published benchmark, record:

1. CPU, memory, storage, OS, compiler/build mode
2. Dataset source, size, document count, average document length
3. Analyzer and ranking configuration
4. Warm-up policy, run duration, concurrency, and number of repetitions
5. Whether the result is a median, percentile, or aggregate
6. The exact command or script used to reproduce it

> ❗ Avoid comparing numbers from different machines or datasets as if they were directly equivalent. No performance figures are claimed in this README until they are measured and published with this context.

---

## 🔒 Security and Privacy

Even a local search engine needs **safe input handling and careful data management**.

- 🔐 Validate document sizes, field lengths, IDs, and query lengths
- 🚫 Treat document content and metadata as untrusted input
- 🔒 Avoid shelling out with untrusted strings
- 💉 Use parameterized database operations where applicable
- ⚡ Set resource limits for parsing and query execution
- 🙈 Do not log full document bodies or sensitive queries by default
- 🔑 Keep credentials and private datasets out of source control
- 🌐 If a network API is added, define auth, TLS, and rate-limiting before exposing it beyond localhost
- 🕷️ Respect source-site terms and crawling policies if a crawler is introduced

---

## 👩‍💻 Development Workflow

### Coding Principles

- Keep the core index and query engine **independent from the API and UI**
- Prefer clear interfaces and **explicit ownership of resources**
- Use RAII and standard library containers where appropriate
- Document non-obvious **invariants and concurrency assumptions**
- Add tests with each feature and **regression tests for every fixed bug**
- Avoid premature optimization; **benchmark before and after** performance changes
- Keep errors **explicit and actionable**

### Contribution Flow

```
Open Issue ──► Discuss Design ──► Create Branch ──► Implement ──► Add Tests ──► Open PR
```

1. 🐛 Open an issue describing the feature, bug, or design question
2. 💬 Discuss changes that affect index formats, query semantics, or public APIs before implementation
3. 🌿 Create a focused branch and make a small, reviewable change
4. 🧪 Add or update tests and documentation
5. 🔨 Run formatting, build, tests, and relevant benchmarks
6. 🔃 Open a pull request with a summary, design rationale, test evidence, and compatibility notes

---

## 📐 Design Decisions

Important architectural decisions should be recorded in `docs/design-decisions.md` using short **Architecture Decision Records (ADRs)**. Each ADR should include:

| Field | Description |
|-------|-------------|
| **Context** | The problem and surrounding circumstances |
| **Options** | Alternatives considered |
| **Decision** | The chosen option and rationale |
| **Trade-offs** | Consequences and trade-offs accepted |
| **Status** | `proposed` · `accepted` · `superseded` |

Potential early ADR topics:

- Document ID and shard-routing strategy
- Tokenization and analyzer configuration
- Posting-list representation
- Segment and on-disk format
- Query syntax and Boolean semantics
- Ranking formula and evaluation corpus
- RPC protocol and timeout policy
- Replication and consistency model

---

## ⚠️ Limitations

> This is an **educational and portfolio engineering project** under development.

The planned architecture is not a substitute for a completed implementation, and features in the roadmap should not be assumed to exist until they are present in code and covered by tests.

Early versions are expected to have:
- Limited language analysis and query syntax
- Limited corpus size and operational tooling
- Limited distributed failure handling

> ❗ Do not index sensitive or production-critical data without reviewing the implementation's security, durability, and access controls.

---

## 🤝 Contributing

Contributions, bug reports, design discussions, and suggestions are welcome!

- Please include **enough detail to reproduce bugs**
- Avoid submitting **large architectural changes** without first discussing their impact
- Check current issues and project documentation for **active work and conventions**

A `CONTRIBUTING.md` file can be added once the contribution process is established.

---

## 📄 License

> ⚠️ No license has been selected yet. Until a `LICENSE` file is added, all rights remain with the copyright holder and reuse, redistribution, or modification is **not automatically granted**.

Add a `LICENSE` file with the chosen open-source license before inviting external contributions.

---

<div align="center">

<br/>

**Built with passion and precision — AskYourX**

<br/>

*If you find this project interesting, consider leaving a ⭐!*

[![GitHub stars](https://img.shields.io/github/stars/your-username/AskYourX?style=social)](https://github.com/your-username/AskYourX)
[![GitHub forks](https://img.shields.io/github/forks/your-username/AskYourX?style=social)](https://github.com/your-username/AskYourX/fork)

<br/>

---

*© 2026 AskYourX. All documentation and content rights reserved.*

</div>
