# DevDocs AI --- Architecture

**Document status:** Approved architecture baseline\
**Scope:** MVP architecture and technical boundaries\
**Architecture style:** Modular monolith, separately deployable frontend
and backend, independently deployable ingestion worker\
**Primary language:** TypeScript

## 1. Purpose

This document defines the technical architecture of DevDocs AI: its
components, responsibilities, data flow, security boundaries, deployment
model, integrations, and operational behavior.

The central requirement is that answers about a repository must be
grounded in retrievable repository evidence and provide verifiable
citations.

DevDocs AI is a repository-understanding application, not an autonomous
coding agent. It will inspect and explain codebases, retrieve evidence,
and answer questions. It will not modify repositories, create commits,
or open pull requests.

### 1.1 Decision classifications

-   **Confirmed:** Explicitly selected during architecture discussions.
-   **Provisional:** Recommended baseline approved for planning, subject
    to technical validation.
-   **Deferred:** Intentionally postponed until a technical spike,
    benchmark, or implementation requirement provides sufficient
    evidence.

A provisional choice must not silently become a permanent dependency
without validation.

## 2. Architecture principles

1.  **Evidence before generation:** Retrieve relevant repository
    evidence before asking a model to answer.
2.  **Verifiable citations:** Citations correspond to stored source
    evidence, line ranges, and the indexed commit.
3.  **User isolation:** Repository data, indexing jobs, and
    conversations remain isolated by account.
4.  **Secure by default:** Repository content is untrusted input.
    Secrets, prompt injection, authorization boundaries, and data
    exposure are addressed throughout the pipeline.
5.  **Modular monolith first:** Keep the initial application
    straightforward while preserving clear module boundaries.
6.  **Independent deployment boundaries:** Frontend, API, and ingestion
    worker are independently deployable even if the API and worker
    initially share infrastructure.
7.  **Portable foundations:** Prefer standard PostgreSQL and small
    provider adapters over unnecessary provider-specific coupling.
8.  **Measured optimization:** Introduce complex retrieval,
    infrastructure, or AI techniques only when evaluation demonstrates a
    need.
9.  **Incremental delivery:** Implement the MVP in stages without
    weakening security, citation integrity, or data isolation.
10. **No unsupported claims:** Distinguish repository evidence from
    general explanations and acknowledge insufficient evidence.

## 3. System overview

### 3.1 High-level architecture

The application consists of: - React + Vite frontend. - Fastify API
using TypeScript. - PostgreSQL database with vector storage. -
Background ingestion worker. - GitHub integration. - Chat-model and
embedding-provider integrations. - Authentication and session
management. - Observability and evaluation components.

``` mermaid
flowchart TD
    U[User] --> FE[React + Vite Frontend]
    FE --> API[Fastify API]
    API --> AUTH[Authentication Provider]
    API --> DB[(PostgreSQL)]
    API --> JOBS[Job Queue]
    API --> AI[AI Provider Adapter]
    JOBS --> WORKER[Ingestion Worker]
    WORKER --> GH[GitHub API / Repository Archive]
    WORKER --> SCAN[File Filtering and Secret Scanning]
    SCAN --> PARSE[Parsing and Structural Extraction]
    PARSE --> CHUNK[Chunking]
    CHUNK --> EMBED[Embedding Provider]
    EMBED --> DB
    AI --> MODEL[Chat Model]
    DB --> RET[Hybrid Retrieval]
    RET --> AI
    AI --> API
    API --> FE
```

### 3.2 Component responsibilities

  -----------------------------------------------------------------------
  Component                           Responsibility
  ----------------------------------- -----------------------------------
  Frontend                            User interaction, repository
                                      management, chat interface,
                                      streaming responses

  API                                 Authentication verification,
                                      authorization, business logic, REST
                                      endpoints, job submission

  Database                            Application records, repository
                                      metadata, conversations, indexing
                                      metadata, chunks and vectors

  Worker                              Repository ingestion, parsing,
                                      chunking, embedding, indexing and
                                      progress reporting

  GitHub integration                  Public repository metadata and
                                      source retrieval

  AI integration                      Chat generation, embeddings and
                                      background summarization

  Queue                               Background job scheduling, retry
                                      handling, concurrency and progress

  Observability                       Logs, metrics, health checks,
                                      errors and operational diagnostics
  -----------------------------------------------------------------------

## 4. Technology architecture

### 4.1 Frontend

**Confirmed** - React. - Vite. - TypeScript. - Separate frontend
deployment. - Cloudflare Pages is the preferred frontend hosting
candidate.

The frontend communicates with the backend through REST endpoints under
`/api/v1`. Chat responses use Server-Sent Events (SSE). The frontend
must not contain privileged database credentials, model-provider
secrets, or GitHub integration secrets.

### 4.2 Backend

**Confirmed** - Node.js, TypeScript and Fastify. - REST API and modular
monolith. - Feature-based modules with internal layers. - API route
schemas as the source of truth. - Generated OpenAPI documentation.

The backend handles authentication verification, authorization, business
logic, repository operations, chat orchestration, and queue submission.

### 4.3 Database

**Confirmed** - PostgreSQL. - Vector storage in the same PostgreSQL
database, using a separate schema where appropriate. - Versioned
migrations. - Explicit index-version tracking. - Database-level
isolation where supported, alongside backend authorization.

**Provisional** - Supabase is the leading integrated database and
authentication candidate. - Neon is the principal alternative. - AWS
remains an alternative when infrastructure control or operational
requirements justify it.

The final database provider and connection model must be validated
before implementation.

### 4.4 Vector retrieval

**Confirmed** - PostgreSQL with pgvector as the preferred baseline. -
Hybrid keyword and vector retrieval. - Reciprocal Rank Fusion (RRF) for
combining ranked result sets. - No reranking in the initial MVP unless
evaluation demonstrates a need.

### 4.5 Queue and background processing

**Confirmed** - Background ingestion. - Global and per-user concurrency
limits. - One worker initially. - Clear internal pipeline stages. -
Bounded retries and backoff. - Manual retry for failed operations. -
Cancellation at supported pipeline boundaries.

**Provisional** - Evaluate BullMQ with PostgreSQL as the first queue
candidate. - Compare against Redis/BullMQ and other PostgreSQL-backed
approaches.

Redis must not be introduced merely because it is a common queue
dependency. Its operational and cost benefits must justify the
additional service.

### 4.6 AI providers

**Confirmed** - Provider abstraction. - One initial provider for each
required capability. - Separate chat and embedding configuration where
appropriate. - A capable chat model and a cheaper model for background
tasks are the preferred pattern.

**Provisional** - Benchmark two or three chat models. - Compare
embedding providers and models. - Select based on quality, evidence
correctness, latency, context capacity, structured output reliability,
and cost.

No final model-provider commitment is made by this document.

### 4.7 Storage

**Confirmed** - Hybrid storage strategy. - Worker-local temporary
storage for intermediate files. - Object storage only when durable
persistence is necessary.

**Deferred** - Whether object storage is required for the MVP. - Final
provider selection; Cloudflare R2 is a candidate.

Temporary files must be cleaned up after processing, including failed
and cancelled jobs.

## 5. Deployment architecture

### 5.1 Frontend deployment

The React + Vite frontend is intended to be deployed on Cloudflare
Pages. Deployment should support production and preview deployments,
environment-specific configuration, and secure API communication.

### 5.2 Backend deployment

The Fastify API will run in a cloud environment supporting a
continuously running Node.js process. Railway and Render are the initial
hosting candidates. AWS remains an alternative.

### 5.3 Worker deployment

The ingestion worker is a separately deployable process. Initially, the
API and worker may share a machine or hosting environment to reduce
infrastructure cost. Their process boundaries, configuration, and
deployment responsibilities must remain separate.

This allows independent scaling or migration later without redesigning
the application.

### 5.4 Docker and builds

The user's local development machine cannot run Docker reliably.
Therefore: - Local Docker must not be a development prerequisite. -
Docker images may be built in managed cloud build infrastructure or
GitHub Actions. - Images may be pushed to a container registry and
deployed to the selected runtime. - The final build/deployment mechanism
remains provisional until hosting comparison.

### 5.5 Development environments

Use isolated logical environments for development and production.
Introduce staging when the deployment workflow requires it.

Database, cache, and other infrastructure dependencies should be
cloud-hosted rather than requiring local containers.

### 5.6 Deployment topology

``` mermaid
flowchart TD
    USER[User Browser] --> CF[Cloudflare Pages]
    CF --> API[Fastify API Runtime]
    API --> AUTH[Auth Provider]
    API --> PG[(Managed PostgreSQL)]
    API --> Q[Queue]
    Q --> W[Ingestion Worker Runtime]
    W --> GH[GitHub]
    W --> AI[Embedding Provider]
    W --> PG
    API --> LLM[Chat Model Provider]
    API --> PG
```

The diagram represents logical boundaries. It does not require every
component to run on separate infrastructure initially.

## 6. Backend module structure

The backend will use feature-based modules with internal layers.

  -----------------------------------------------------------------------
  Module                              Responsibility
  ----------------------------------- -----------------------------------
  `auth`                              Session verification, identity and
                                      authentication integration

  `users`                             User profile and account lifecycle

  `repositories`                      Add, view, refresh and delete
                                      repository records

  `github`                            GitHub API and source-retrieval
                                      integration

  `ingestion`                         Ingestion orchestration and stage
                                      coordination

  `parsing`                           File filtering, parsing and
                                      structural extraction

  `indexing`                          Chunking, embedding and
                                      index-version management

  `retrieval`                         Keyword search, vector search and
                                      RRF

  `chat`                              Conversations, context management
                                      and response orchestration

  `ai`                                Chat and embedding provider
                                      adapters

  `jobs`                              Queue operations, retries,
                                      cancellation and progress

  `observability`                     Logging, metrics, health checks and
                                      error tracking
  -----------------------------------------------------------------------

### 6.1 Internal layering

Separate responsibilities where useful: - **Routes:** HTTP request and
response handling. - **Application/services:** Use-case orchestration. -
**Domain:** Business rules and core concepts. - **Infrastructure:**
Database, external APIs, queue and provider integrations.

Do not introduce interfaces or design patterns without a concrete
reason. SOLID principles should improve maintainability rather than
produce unnecessary abstraction.

### 6.2 Dependency direction

Business logic should not depend directly on provider-specific SDK
behavior. External services should be accessed through small integration
boundaries where this materially improves portability or testability.

## 7. Authentication and authorization

### 7.1 Authentication

**Confirmed** - Account-based application. - Provider-managed sessions
are preferred. - Email verification is required before expensive
operations. - Account recovery should be supported only when the
selected provider supports it securely.

**Deferred** - Final authentication provider. - Exact authentication
method and recovery implementation.

### 7.2 Authorization

The backend must authenticate the user and authorize every
repository-specific operation.

Every repository, indexing job, conversation, message, and
source-evidence lookup must be scoped to the authenticated user. The
backend must never trust a user ID supplied by the frontend as proof of
ownership.

### 7.3 Database isolation

Database-level isolation should be used where supported by the selected
provider and connection model.

RLS is a preferred capability to evaluate, not an assumption that
automatically guarantees security. If the backend uses privileged
database credentials or connections that bypass RLS, explicit ownership
checks remain mandatory.

### 7.4 User isolation model

-   One authenticated user owns their repository records.
-   Each user's public repository record is isolated from other users'
    records.
-   Public repositories may be indexed more than once for different
    users.
-   Shared indexing across users is out of scope.
-   Conversations belong to a user and a repository.
-   Jobs must be scoped to the owning user and repository.

## 8. Repository ingestion architecture

### 8.1 Repository submission

The user submits a public GitHub repository URL. The API: 1. Validates
the URL and supported repository format. 2. Retrieves repository
metadata. 3. Checks configured repository limits and eligibility. 4.
Presents repository details for confirmation where required. 5. Creates
the repository record and indexing job. 6. Returns the job status to the
frontend.

### 8.2 Source retrieval

Use a hybrid GitHub retrieval approach: - GitHub API for metadata and
targeted repository operations. - Repository archive download or Git
retrieval for bulk source ingestion. - Pin every indexing operation to a
specific commit SHA. - Respect GitHub rate limits and response
headers. - Use bounded retries and backoff for temporary failures.

The precise API-versus-archive strategy should be validated during
implementation.

### 8.3 File filtering and safety

The ingestion pipeline must: - Apply configurable repository size
limits. - Apply configurable file-count and individual-file-size
limits. - Allow supported source, documentation, configuration, and API
specification files. - Exclude unsupported or irrelevant content. -
Detect and exclude secrets. - Avoid logging source contents or secret
values. - Record skipped and failed files for diagnostics.

The exact allowlist, exclusion rules, and limits remain configurable and
must be finalized during MVP planning.

### 8.4 Language support

Initial structural-analysis languages: - JavaScript. - TypeScript. -
Python.

Tree-sitter is the preferred parsing approach, with language-specific
extraction. The application must still support language-agnostic text
retrieval for files that do not receive enhanced structural analysis.

### 8.5 Structural extraction

Where supported, extract functions, classes, modules, imports, symbol
relationships, and relevant API and configuration structures.

Structural extraction must not become a prerequisite for all indexing.
Token-based chunking provides a fallback when parsing is unavailable or
unsuccessful.

### 8.6 Ingestion pipeline

``` mermaid
flowchart TD
    A[Repository Submitted] --> B[Validate URL and Metadata]
    B --> C[Create Pinned-Commit Job]
    C --> D[Retrieve Repository Source]
    D --> E[Apply File and Safety Filters]
    E --> F[Secret Detection]
    F --> G[Language Detection]
    G --> H[Tree-sitter Parsing]
    H --> I[Symbol and Relationship Extraction]
    I --> J[Hybrid Chunk Creation]
    J --> K[Generate Embeddings]
    K --> L[Persist Chunks and Metadata]
    L --> M[Build Repository Summary]
    M --> N{Index Result}
    N -->|Sufficient usable content| O[Ready or Partially Ready]
    N -->|Insufficient usable content| P[Failed or Not Ready]
```

### 8.7 Job stages and retry behavior

The worker should expose simplified user-facing job status and
stage-level internal progress.

The initial retry approach is whole-job retry with clear stage
boundaries. Stages must support safe re-execution through idempotent
operations and explicit index-version boundaries.

Temporary failures should receive bounded retries with backoff.
Permanent failures should be classified and reported without endless
retry loops.

### 8.8 Indexing result

An indexing job can produce: - **Ready:** Enough usable content was
indexed. - **Partially ready:** Enough content was indexed to be useful,
but some content was skipped or failed. - **Failed/not ready:**
Insufficient usable content or an unrecoverable failure.

Thresholds must be configurable and validated against representative
repositories.

## 9. Index and revision management

### 9.1 Commit pinning

Every index must identify the repository commit SHA from which its
evidence was extracted. A source citation must refer to the correct
indexed revision.

### 9.2 Refresh

A refresh indexes the current default-branch revision. If a refresh
begins while another revision is being indexed, the existing job should
finish against its pinned commit. A subsequent refresh is handled as a
separate operation.

### 9.3 Historical evidence

The system should preserve enough revision and evidence metadata to
identify the source used by existing conversations. Existing
conversations must not silently begin referring to a different revision
after refresh.

### 9.4 Index version metadata

Track repository revision, index version, embedding model and version,
processing status, relevant extraction configuration, and evidence
identifiers.

The exact retention period and cleanup policy remain deferred.

## 10. Retrieval and indexing architecture

### 10.1 Chunking

Use hybrid structural and token-based chunking. Prefer meaningful code
structures when available, such as functions, classes, and modules. Use
token-based fallback chunking when structural parsing is unavailable.

Chunks must retain sufficient source metadata to support citation
validation.

### 10.2 Embeddings

Embedding generation should: - Process only usable chunks. - Use
batching where supported. - Enforce token and cost budgets. - Track
embedding model and version. - Support re-embedding when model or index
configuration changes.

### 10.3 Hybrid retrieval

The initial retrieval design combines keyword retrieval, vector
similarity retrieval, and Reciprocal Rank Fusion.

RRF combines ranked result lists. Reranking is deferred until retrieval
evaluation demonstrates a meaningful improvement.

### 10.4 Retrieval context

Context construction must account for retrieved chunks, source metadata,
conversation context, token budgets, evidence relevance and diversity.

Do not pass an unbounded repository or conversation history into the
model.

### 10.5 Repository summaries

Start with a repository-level summary. Directory- and module-level
summaries may be added later if evaluation shows that they improve
cross-file understanding.

## 11. Chat and answer generation

### 11.1 Chat flow

``` mermaid
sequenceDiagram
    participant U as User
    participant FE as Frontend
    participant API as Fastify API
    participant DB as PostgreSQL
    participant R as Retrieval
    participant AI as Chat Model

    U->>FE: Submit question
    FE->>API: Chat request
    API->>DB: Authenticate, authorize, load context
    API->>R: Retrieve repository evidence
    R->>DB: Keyword and vector search
    DB-->>R: Ranked evidence
    R-->>API: Validated evidence
    API->>AI: Bounded prompt with evidence
    AI-->>API: Generated response
    API->>DB: Persist response and citations
    API-->>FE: SSE response
    FE-->>U: Display answer and sources
```

### 11.2 Evidence requirements

The model must: - Answer using retrieved repository evidence when making
repository-specific claims. - Use source references supplied by the
backend. - Avoid inventing file paths, line ranges, symbols, or commit
identifiers. - Clearly state when repository evidence is insufficient. -
Separate general technical explanations from repository-specific facts.

### 11.3 Citations

Each citation should contain or resolve to repository identity, file
path, validated line range, indexed commit SHA, and evidence identifier.

The backend must validate citation references against stored evidence
before presenting them as verified sources. Where feasible, GitHub links
should be pinned to the indexed commit.

### 11.4 Follow-up questions

Follow-up questions should retain conversation context while performing
fresh retrieval.

Use token-budgeted rolling context or summarization when necessary. Do
not assume that evidence retrieved for an earlier question is sufficient
for every follow-up.

### 11.5 Streaming and cancellation

Use SSE for chat response streaming.

If a user cancels generation: - Stop work at supported cancellation
boundaries. - Preserve the partial response. - Mark the message as
incomplete or cancelled. - Avoid presenting a partial answer as a
completed answer.

## 12. API architecture

### 12.1 API style

**Confirmed** - REST. - `/api/v1` prefix. - Fastify route schemas as the
source of truth. - Generated OpenAPI documentation. - Standardized API
error format.

Shared types and generated OpenAPI types may be used together, but
competing sources of truth must be avoided.

### 12.2 Initial API surface

The following is a conceptual MVP route inventory. Exact request and
response schemas are to be finalized in implementation planning.

  ---------------------------------------------------------------------------------------------------------
  Area                    Route                                                     Purpose
  ----------------------- --------------------------------------------------------- -----------------------
  Repositories            `POST /api/v1/repositories`                               Add a repository

  Repositories            `GET /api/v1/repositories`                                List user's
                                                                                    repositories

  Repositories            `GET /api/v1/repositories/:repositoryId`                  Repository details and
                                                                                    status

  Repositories            `POST /api/v1/repositories/:repositoryId/refresh`         Start a refresh

  Repositories            `DELETE /api/v1/repositories/:repositoryId`               Request repository
                                                                                    deletion

  Jobs                    `GET /api/v1/jobs/:jobId`                                 Read job status

  Jobs                    `POST /api/v1/jobs/:jobId/retry`                          Retry an eligible
                                                                                    failed job

  Conversations           `GET /api/v1/repositories/:repositoryId/conversations`    List repository
                                                                                    conversations

  Conversations           `POST /api/v1/repositories/:repositoryId/conversations`   Create a conversation

  Chat                    `POST /api/v1/conversations/:conversationId/messages`     Submit a chat message

  Chat                    `GET /api/v1/conversations/:conversationId/messages`      Retrieve conversation
                                                                                    messages

  Chat                    `GET /api/v1/conversations/:conversationId/stream`        Stream a response where
                                                                                    SSE uses a dedicated
                                                                                    endpoint
  ---------------------------------------------------------------------------------------------------------

The final route shape, HTTP methods, payloads, and error contracts must
be reviewed against the frontend and backend implementation plan.

### 12.3 Error handling

Use a consistent error response structure containing a stable error
code, safe human-readable message, request or correlation identifier,
and optional structured validation details.

Do not expose stack traces, secrets, provider credentials, or internal
source content in user-facing errors.

## 13. Conceptual data architecture

PostgreSQL is the primary application data store. Vector data should
reside in the same database, using a separate schema where appropriate.

The following entities describe the conceptual model. Exact column
types, indexes, constraints, foreign keys, and migration details must be
validated during schema design.

  -----------------------------------------------------------------------
  Entity                              Purpose
  ----------------------------------- -----------------------------------
  Users                               Application user identity and
                                      provider mapping

  Repositories                        User-owned repository metadata and
                                      status

  Repository revisions                Commit-pinned revision records

  Index versions                      Processing and embedding
                                      configuration versions

  Ingestion jobs                      Job status, stages, attempts and
                                      diagnostics

  Repository files                    File path, metadata, hash and
                                      revision association

  Symbols                             Extracted functions, classes and
                                      other structural elements

  Chunks                              Retrieval units with source
                                      metadata

  Embeddings                          Vector representation of indexed
                                      chunks

  Conversations                       Repository-specific user
                                      conversations

  Messages                            User and assistant messages,
                                      including completion state

  Citations                           References connecting answers to
                                      stored evidence
  -----------------------------------------------------------------------

### 13.1 Data ownership

Every user-owned resource must have an enforceable ownership
relationship.

Queries must scope repository-specific data through the authenticated
user and owned repository. Never rely solely on frontend filtering for
isolation.

### 13.2 Deletion

Repository deletion should: 1. Mark the repository as pending deletion.
2. Prevent new operations against it. 3. Schedule asynchronous cleanup.
4. Remove repository-specific conversations and indexed data according
to the deletion policy. 5. Record cleanup completion or failure. 6.
Expose a clear final status.

Account deletion must coordinate authentication-provider deletion and
application-data cleanup. These operations may be asynchronous and must
not be represented as complete before cleanup is confirmed.

### 13.3 Historical data

Preserve evidence needed by existing conversations. The exact retention
and cleanup policy remains deferred, but retention must be bounded and
deletion behavior must be documented.

## 14. Security architecture

### 14.1 Untrusted repository content

Repository files, comments, documentation, configuration, and source
code are untrusted data.

The application must distinguish repository content from system
instructions and user instructions.

Prompt defenses should include: - Explicit untrusted-content
boundaries. - Restricted tool permissions. - Validated tool arguments. -
Prompt-injection tests. - No execution of repository-provided
instructions as privileged commands.

### 14.2 Secrets

Use pattern-based scanning and a dedicated secret scanner where
practical.

Secrets must be excluded or redacted before persistence and must never
be written into ordinary application logs.

### 14.3 Authentication and authorization

-   Verify sessions on protected backend operations.
-   Derive the acting user from verified authentication context.
-   Enforce ownership on every repository, job, conversation, and
    evidence operation.
-   Apply database-level isolation where supported.
-   Keep privileged credentials exclusively on the backend.

### 14.4 Cost and abuse safeguards

Implement API rate limits, per-user and global indexing limits, separate
chat and indexing limits, token and cost budgets, concurrency controls,
clear limit messages and reset timing, and graceful rejection when a
budget is exhausted.

### 14.5 Configuration and secrets management

Local configuration may use environment variables.

Production secrets must be stored in the selected deployment platform's
secrets-management facility. Never commit secrets to source control.

## 15. Observability and operational behavior

### 15.1 Logging

Use structured logs with relevant diagnostic information: - Request
ID. - User or resource identifiers where safe. - Job and stage
identifiers. - Model and embedding version. - Token usage. - Latency. -
Status and error classification. - Source identifiers and retrieval
ranking diagnostics.

Avoid logging full repository contents, prompts containing sensitive
data, secrets, or unnecessary personal data.

### 15.2 Metrics

Initial operational metrics should include API request counts and
latency, error rates, ingestion duration and stage failures, queue depth
and job age, retry counts, token usage and model costs, retrieval
performance, chat response latency, and citation validation failures.

### 15.3 Health checks

Provide health checks for the API and worker, including relevant
dependency readiness where appropriate.

### 15.4 Failure handling

  -----------------------------------------------------------------------
  Failure                             Expected behavior
  ----------------------------------- -----------------------------------
  GitHub rate limit                   Respect reset information, back off
                                      and report waiting status

  Temporary provider error            Bounded retry with backoff

  Permanent ingestion error           Mark failed and expose actionable
                                      diagnostics

  Partial file processing             Preserve successful evidence and
                                      report skipped/failed content

  Insufficient usable content         Mark not ready rather than
                                      presenting an empty index as
                                      successful

  Model provider failure              Return a clear temporary error; no
                                      automatic provider failover
                                      initially

  User cancellation                   Stop at supported boundaries and
                                      preserve incomplete status

  Cleanup failure                     Keep deletion pending and expose
                                      the failure for recovery
  -----------------------------------------------------------------------

## 16. Testing and evaluation architecture

Testing must cover more than ordinary API behavior.

### 16.1 Component and integration tests

Cover API routes and validation, authentication and ownership checks,
database constraints and migrations, repository ingestion stages, queue
behavior and retries, cancellation, retrieval and citation validation,
SSE behavior, deletion and cleanup.

### 16.2 Security tests

Cover cross-user data access attempts, unauthorized repository and
conversation access, prompt-injection attempts in repository content,
malicious or unexpected repository files, secret detection and log
redaction, rate-limit and budget enforcement, and invalid tool arguments
and source references.

### 16.3 Retrieval and answer evaluation

Build a benchmark using representative repositories and questions.

Evaluate retrieval relevance, evidence coverage, code explanation
correctness, cross-file flow accuracy, citation path and line-range
correctness, unsupported-claim rate, structured response validity,
latency and cost.

Numerical performance thresholds should be set after a baseline is
measured. Do not claim performance improvements without repeatable
evaluation.

## 17. Scalability and performance

The first version targets a small number of concurrent users, with exact
scale to be measured.

The initial architecture should use: - A mostly stateless API. - A
separately deployable worker. - Global and per-user concurrency
limits. - Bounded job and model budgets. - Small in-memory caching where
useful. - Database-backed persistence for application state.

Avoid premature microservices, distributed caches, complex agent
orchestration, or automatic model failover.

Scale API and worker independently when observed workload requires it.

## 18. Deferred technical decisions

  -----------------------------------------------------------------------
  Decision                Current position        Required validation
  ----------------------- ----------------------- -----------------------
  Database and auth       Supabase preferred;     Integration, RLS,
  provider                Neon alternative        pooling, migration and
                                                  cost spike

  InsForge                Candidate under         Verify operational
                          evaluation              capabilities and
                                                  documentation

  Database connection     Provider-dependent      Connection limits,
  model                                           pooling and backend
                                                  access

  Queue                   BullMQ with PostgreSQL  Retry, concurrency and
                          is first candidate      provider compatibility
                                                  test

  Redis                   Alternative if          Demonstrate a concrete
                          justified               operational benefit

  Backend hosting         Railway and Render      Persistent worker, cost
                          candidates              and deployment test

  AWS hosting             Alternative             Justify additional
                                                  infrastructure

  Chat model              Not selected            Fixed benchmark

  Embedding model         Not selected            Retrieval quality and
                                                  cost benchmark

  Object storage          Not required by default Demonstrate durable
                                                  storage need

  Repository limits       Configurable            Representative
                                                  repository measurements

  File allowlist          Configurable            Security and ingestion
                                                  testing

  Partial-index           Configurable            Baseline and evaluation
  thresholds                                      

  Historical evidence     Preserve evidence       Define bounded
  retention               needed by conversations retention and cleanup

  RLS implementation      Provider-dependent      Validate actual
                                                  connection and
                                                  privilege model

  Deployment budget       Free development,       Measure representative
                          low-cost paid           usage
                          production              
  -----------------------------------------------------------------------

## 19. Architecture decision register

This document is the single architecture source of truth. Separate ADR
files are not required at this stage.

  -----------------------------------------------------------------------
  Decision                            Status
  ----------------------------------- -----------------------------------
  Modular monolith                    Confirmed

  React + Vite + TypeScript           Confirmed

  Node.js + Fastify + TypeScript      Confirmed

  Monorepo                            Confirmed

  PostgreSQL                          Confirmed

  pgvector                            Confirmed baseline

  Hybrid retrieval with RRF           Confirmed

  Tree-sitter for JS, TS and Python   Confirmed baseline

  GitHub API + bulk source retrieval  Confirmed strategy; implementation
                                      details provisional

  Commit-pinned indexing              Confirmed

  SSE chat streaming                  Confirmed

  `/api/v1` REST API                  Confirmed

  Cloudflare Pages                    Preferred frontend deployment

  Supabase                            Provisional preferred integrated
                                      platform

  BullMQ PostgreSQL backend           Provisional queue candidate

  Railway / Render                    Provisional backend hosting
                                      candidates

  Cloud-based image builds            Confirmed requirement; provider
                                      deferred

  Object storage                      Deferred unless required

  Chat and embedding models           Deferred pending benchmarks

  RLS details                         Deferred pending provider selection
  -----------------------------------------------------------------------

## 20. Implementation boundary

This document authorizes architecture planning only. It does **not**
authorize application implementation.

Before coding begins, the remaining required documentation must be
completed and reviewed:

1.  `build-plan.md`
2.  `code-standards.md`
3.  `library-docs.md`
4.  `ui-tokens.md`
5.  `ui-rules.md`
6.  `ui-registry.md`
7.  `progress-tracker.md`

The implementation process must follow the documentation-first
instructions: inspect the repository, load the relevant documentation,
work only on an authorized task, make incremental changes, run
applicable checks, and report actual results.

Major architectural changes, new dependencies, destructive operations,
and changes to approved scope require review before proceeding.

------------------------------------------------------------------------

**End of architecture document.**
