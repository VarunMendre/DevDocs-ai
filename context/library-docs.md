
# DevDocs AI — Library Documentation

## 1. Purpose

This document defines the technology stack, libraries, frameworks, development tools, and external services intended for DevDocs AI.

It serves as the central reference for:
- Technologies selected for the project.
- Libraries and providers under evaluation.
- The intended responsibility of each technology.
- Usage boundaries and integration guidelines.
- Security and reliability considerations.
- Official documentation references.
- Dependency selection and version-management rules.

The purpose is to ensure consistent technology usage across the frontend, backend, background worker, database, and deployment infrastructure.

This document must remain consistent with:
- `project-overview.md`
- `architecture.md`
- `build-plan.md`
- `code-standards.md`

---

## 2. Technology Selection Status

Every technology must be classified using one of the following statuses:

| Status | Meaning |
|---|---|
| Selected | Approved as part of the project architecture. |
| Provisional | Preferred candidate, but final adoption requires verification. |
| Under Evaluation | Requires comparison before selection. |
| Alternative | Potential replacement if the preferred option is unsuitable. |
| Deferred | Not required for the current MVP. |

A technology must not be treated as selected merely because it appears in this document.

Provider-specific capabilities, pricing, limits, and integration behavior must be verified before adoption.

---

## 3. Core Technology Stack

The following technologies are established by the approved architecture.

| Technology | Purpose | Status |
|---|---|---|
| TypeScript | Primary programming language | Selected |
| React | Frontend application | Selected |
| Vite | Frontend development and build tooling | Selected |
| Node.js | Backend and worker runtime | Selected |
| Fastify | Backend HTTP framework | Selected |
| PostgreSQL | Primary relational database | Selected |
| pgvector | Vector storage and similarity search | Selected |
| REST | Backend API communication | Selected |
| OpenAPI | API contract documentation | Selected |
| Server-Sent Events (SSE) | Streaming AI responses | Selected |
| Monorepo | Shared project organization | Selected |

### 3.1 TypeScript

**Purpose:** Provide static typing across frontend, backend, worker, and shared packages.

Usage guidelines:
- Enable strict TypeScript settings.
- Avoid unnecessary `any` and unsafe type assertions.
- Define clear interfaces and types for module boundaries.
- Validate external data at runtime.
- Share types where doing so avoids duplicated contracts.
- Do not treat compile-time types as runtime validation.

Official documentation:

https://www.typescriptlang.org/docs/

### 3.2 React

**Purpose:** Build the user-facing application.

Usage guidelines:
- Use functional components and hooks.
- Keep components focused on presentation and interaction.
- Use reusable components where appropriate.
- Avoid unnecessary global state.
- Follow approved UI tokens, rules, and component registry.
- Implement accessible and responsive interfaces.

Official documentation:

https://react.dev/

### 3.3 Vite

**Purpose:** Frontend development server and production build tooling.

Usage guidelines:
- Keep environment-specific configuration separate.
- Avoid exposing server secrets through frontend environment variables.
- Use production builds for deployment.
- Configure aliases and build settings consistently.

Official documentation:

https://vite.dev/guide/

### 3.4 Node.js

**Purpose:** Runtime for the backend API and ingestion worker.

Usage guidelines:
- Prefer asynchronous, non-blocking operations.
- Avoid unnecessary synchronous file and network operations.
- Handle process errors and shutdown correctly.
- Bound memory and concurrency.
- Use supported Node.js LTS releases.

Official documentation:

https://nodejs.org/docs/latest/api/

### 3.5 Fastify

**Purpose:** Backend HTTP framework.

Usage guidelines:
- Organize routes by feature.
- Use request and response schemas.
- Keep business logic in services.
- Centralize error handling.
- Use hooks and plugins for cross-cutting concerns.
- Apply appropriate request-size and timeout limits.
- Generate API documentation from the established route schemas.

Official documentation:

https://fastify.dev/docs/latest/

### 3.6 PostgreSQL

**Purpose:** Primary relational database for application data.

Expected responsibilities:
- User-related application data.
- Repository metadata.
- Repository revisions.
- Ingestion jobs.
- Index versions.
- Repository files and symbols.
- Chunks and embeddings.
- Conversations and messages.
- Citations.

Usage guidelines:
- Use migrations for schema changes.
- Apply database constraints.
- Use transactions where atomicity is required.
- Index frequently queried fields based on actual query patterns.
- Use parameterized queries.
- Apply user ownership checks.
- Avoid unbounded queries.

Official documentation:

https://www.postgresql.org/docs/

### 3.7 pgvector

**Purpose:** Store and search vector embeddings within PostgreSQL.

Expected responsibilities:
- Persist embeddings associated with repository chunks.
- Perform vector similarity searches.
- Support semantic retrieval.
- Work alongside PostgreSQL full-text search.

Usage guidelines:
- Track embedding model and dimensions.
- Maintain consistent embedding versions.
- Verify vector dimensions before persistence.
- Evaluate indexing and search performance.
- Do not assume vector similarity alone guarantees relevant retrieval.

Official documentation:

https://github.com/pgvector/pgvector

---

## 4. Database Access and Authentication

The database access and authentication implementation must be selected after comparing provider capabilities and integration requirements.

### 4.1 Supabase

**Purpose:** Candidate for managed PostgreSQL and authentication.

**Status:** Provisional candidate.

Potential use:
- Managed PostgreSQL.
- Authentication integration.
- Database hosting.
- Potential Row-Level Security support.

Official documentation:

https://supabase.com/docs

Evaluation requirements:
- Verify current free-tier and production pricing.
- Verify authentication capabilities and account recovery.
- Verify connection pooling and database access options.
- Verify RLS behavior with the intended backend connection model.
- Verify migration and backup workflows.
- Verify PostgreSQL and pgvector compatibility.

### 4.2 Neon

**Purpose:** Alternative managed PostgreSQL provider.

**Status:** Alternative / Under Evaluation.

Potential use:
- Managed PostgreSQL.
- Database branching and development workflows.
- PostgreSQL hosting independent of the authentication provider.

Official documentation:

https://neon.com/docs

Evaluation requirements:
- Compare pricing and compute limits.
- Verify connection management.
- Verify pgvector support.
- Review database branching and migration workflows.
- Evaluate integration with the selected authentication provider.

### 4.3 InsForge

**Purpose:** Candidate integrated backend platform.

**Status:** Under Evaluation.

Evaluation requirements:
- Verify current PostgreSQL support.
- Verify authentication and authorization capabilities.
- Verify database access and connection behavior.
- Verify supported deployment and integration workflows.
- Compare operational limitations and pricing.
- Verify compatibility with the approved architecture.

Official website:

https://insforge.dev/

Do not assume capabilities without consulting current official documentation.

### 4.4 AWS Database and Authentication Alternatives

AWS services remain potential alternatives where they provide a clear operational or architectural advantage.

**Status:** Alternative.

Potential services:
- Amazon RDS for PostgreSQL.
- Amazon Cognito for authentication.
- AWS Secrets Manager for secret storage.

Official documentation:

https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html

https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-userpools.html

https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html

Do not introduce AWS infrastructure unnecessarily if a simpler managed provider satisfies the requirements.

### 4.5 Selection Principle

The database and authentication providers may be separate or integrated.

The final selection must consider:
- Security.
- Data isolation.
- Developer experience.
- PostgreSQL compatibility.
- Authentication capabilities.
- Operational complexity.
- Free development availability.
- Low-cost production suitability.
- Portability and migration options.

Provider-specific behavior must be hidden behind appropriate application boundaries where practical.

---

## 5. Input Validation and Schema Management

### 5.1 Zod

**Purpose:** Runtime validation and schema definition.

**Status:** Provisional preferred candidate.

Potential use:
- API request validation.
- Environment-variable validation.
- External API response validation.
- Structured model-output validation.
- Shared frontend/backend contracts where appropriate.

Usage guidelines:
- Define schemas at application boundaries.
- Reuse schemas where contracts genuinely match.
- Avoid duplicating validation logic.
- Return clear validation errors.
- Never rely solely on frontend validation.

Official documentation:

https://zod.dev/

The final version and integration strategy must be verified before adoption.

---

## 6. Database ORM and Query Layer

The project requires a consistent database access strategy.

### 6.1 Prisma

**Purpose:** Candidate ORM for PostgreSQL access, migrations, and type-safe database operations.

**Status:** Under Evaluation.

Potential benefits:
- Typed database client.
- Schema-based data modeling.
- Migration workflows.
- Developer-friendly database access.

Official documentation:

https://www.prisma.io/docs

Evaluation requirements:
- Verify compatibility with the selected PostgreSQL provider.
- Verify pgvector integration and required raw SQL capabilities.
- Evaluate connection pooling.
- Review transaction behavior.
- Evaluate migration reliability.
- Avoid unnecessary ORM abstractions.

### 6.2 Alternatives

Other PostgreSQL access approaches may be evaluated if they better support:
- pgvector queries.
- Full-text search.
- Complex SQL.
- Migration workflows.
- Connection management.

The final choice must avoid unnecessary complexity and preserve access to PostgreSQL capabilities.

---

## 7. Background Job Processing

Repository ingestion and indexing must execute asynchronously.

### 7.1 PostgreSQL-Backed Queue

**Purpose:** Candidate approach for background job processing.

**Status:** Under Evaluation.

Potential benefits:
- Fewer infrastructure dependencies.
- Simplified development environment.
- Persistence using the existing database.

Evaluation requirements:
- Verify concurrency handling.
- Verify job claiming and locking.
- Verify retry behavior.
- Verify failure recovery.
- Verify scheduling and monitoring capabilities.

### 7.2 BullMQ and Redis

**Purpose:** Candidate background job queue.

**Status:** Provisional candidate, subject to comparison.

Potential use:
- Repository ingestion.
- Index generation.
- Embedding generation.
- Retry management.
- Job scheduling and concurrency control.

Official documentation:

https://docs.bullmq.io/

Redis documentation:

https://redis.io/docs/latest/

Evaluation requirements:
- Verify current BullMQ and Redis compatibility.
- Evaluate persistence and failure recovery.
- Evaluate concurrency controls.
- Compare operational complexity and hosting costs.
- Confirm whether Redis is genuinely necessary.

**Important:** Redis must not be introduced automatically. A PostgreSQL-backed approach should be preferred if it satisfies the project's requirements without introducing reliability compromises.

---

## 8. Repository Retrieval and Parsing

### 8.1 GitHub API

**Purpose:** Retrieve public repository metadata and related information.

**Status:** Selected as the repository integration direction.

Potential responsibilities:
- Repository metadata retrieval.
- Default branch information.
- Commit information.
- Repository validation.
- Rate-limit-aware requests.

Official documentation:

https://docs.github.com/en/rest

Usage guidelines:
- Support public repositories only for the MVP.
- Validate repository URLs.
- Handle rate limits.
- Use bounded retries and backoff.
- Pin ingestion to a commit SHA.
- Validate external responses.
- Avoid unrestricted URL fetching.

### 8.2 Git and Repository Archive Retrieval

**Purpose:** Retrieve repository source content.

**Status:** Selected as the retrieval approach; exact implementation method remains an engineering choice.

Potential approaches:
- GitHub archive downloads.
- Git-based source retrieval.

Requirements:
- Pin source to a specific commit.
- Enforce repository and file limits.
- Handle interrupted downloads.
- Prevent path traversal.
- Avoid executing repository content.
- Keep temporary storage bounded.

### 8.3 Tree-sitter

**Purpose:** Structural parsing of selected programming languages.

**Status:** Selected architectural direction; exact package integrations require verification.

Initial languages:
- JavaScript.
- TypeScript.
- Python.

Potential responsibilities:
- Function extraction.
- Class extraction.
- Module relationships.
- Structural code metadata.
- Symbol identification.

Official documentation:

https://tree-sitter.github.io/tree-sitter/

Usage guidelines:
- Use language-specific grammars.
- Handle syntax errors and incomplete source files.
- Preserve source locations.
- Do not assume every language is supported.
- Maintain language-agnostic text retrieval as a fallback.

### 8.4 Chunking

**Purpose:** Divide repository content into retrievable units.

**Status:** Selected architectural approach.

The approved architecture uses:
- Hybrid structural chunking.
- Token-based chunking.

Requirements:
- Preserve source paths.
- Preserve line ranges.
- Associate chunks with repository revisions.
- Avoid unnecessary duplication within a repository revision.
- Keep chunks within model and embedding limits.
- Maintain stable evidence identifiers.

Chunking implementation should be evaluated using retrieval quality rather than arbitrary chunk sizes alone.

---

## 9. Embedding and Language Model Providers

Model providers have not been finalized.

Chat and embedding providers must be evaluated separately because they serve different purposes.

### 9.1 Embedding Provider

**Purpose:** Convert repository chunks into vector representations.

**Status:** Under Evaluation.

Selection criteria:
- Code retrieval quality.
- Supported languages.
- Embedding dimensions.
- Context limits.
- Latency.
- Cost.
- Availability.
- Data handling and privacy.
- Ease of model replacement.

Requirements:
- Record model identity and version.
- Track embedding dimensions.
- Re-embed when the selected embedding model changes incompatibly.
- Avoid mixing incompatible embedding spaces.
- Evaluate using representative repository queries.

### 9.2 Chat Model Provider

**Purpose:** Generate repository-grounded explanations and answers.

**Status:** Under Evaluation.

Selection criteria:
- Code understanding.
- Cross-file reasoning.
- Citation accuracy.
- Structured output reliability.
- Context-window capacity.
- Response latency.
- Cost.
- Availability.
- Security and data-handling policies.

Requirements:
- Use retrieved evidence as the basis for repository-specific claims.
- Clearly distinguish general explanations from repository facts.
- Communicate insufficient evidence.
- Validate generated citations.
- Track token usage and model costs.
- Handle provider errors and rate limits.

### 9.3 Model Selection Strategy

Benchmark two or three chat model candidates.

Evaluate:
- Code explanation quality.
- Cross-file flow understanding.
- Evidence correctness.
- Structured output and citation accuracy.
- Latency and cost.

Evaluate embedding candidates separately.

A capable chat model and a cheaper model for background tasks may be used if evaluation supports the approach.

No provider or model should be treated as finalized before benchmarking.

---

## 10. Retrieval and Ranking

### 10.1 PostgreSQL Full-Text Search

**Purpose:** Retrieve repository content through keyword-based matching.

**Status:** Selected architectural direction.

Official documentation:

https://www.postgresql.org/docs/current/textsearch.html

Potential benefits:
- Keyword matching.
- Search over code and documentation.
- Complementary retrieval to semantic search.

### 10.2 Vector Search

**Purpose:** Retrieve semantically related repository chunks.

**Status:** Selected architectural direction through pgvector.

### 10.3 Reciprocal Rank Fusion (RRF)

**Purpose:** Combine keyword and vector retrieval rankings.

**Status:** Selected architectural direction.

Requirements:
- Preserve source metadata.
- Keep retrieval scoring and ranking behavior testable.
- Evaluate ranking quality against representative queries.
- Avoid adding a reranker initially unless evaluation shows a meaningful benefit.

A reranker is deferred until the baseline retrieval system is measured.

---

## 11. API and Streaming

### 11.1 REST API

**Purpose:** Communication between frontend and backend.

**Status:** Selected.

API base path:

`/api/v1`

### 11.2 OpenAPI

**Purpose:** API contract documentation.

**Status:** Selected.

The approved architecture establishes Fastify route schemas as the source of truth and generated OpenAPI documentation.

Official documentation:

https://spec.openapis.org/oas/latest.html

Requirements:
- Keep API schemas consistent with implementation.
- Avoid manually maintaining conflicting API definitions.
- Validate generated API documentation.
- Maintain compatible contracts.

### 11.3 Server-Sent Events

**Purpose:** Stream generated AI responses to the frontend.

**Status:** Selected.

Requirements:
- Handle client disconnection.
- Support cooperative cancellation.
- Handle partial responses.
- Distinguish completed, failed, and cancelled responses.
- Avoid leaking internal model or provider errors.
- Persist response state accurately.

Official documentation:

https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events

---

## 12. Testing and Code Quality Tools

The exact package versions and final test-library selection must be verified before implementation.

### 12.1 ESLint

**Purpose:** Static code analysis and coding-rule enforcement.

**Status:** Selected tool.

Official documentation:

https://eslint.org/docs/latest/

### 12.2 Prettier

**Purpose:** Consistent code formatting.

**Status:** Selected tool.

Official documentation:

https://prettier.io/docs/

### 12.3 TypeScript Compiler

**Purpose:** Static type checking and compilation.

**Status:** Selected tool.

Official documentation:

https://www.typescriptlang.org/docs/

### 12.4 Testing Framework

**Purpose:** Unit, integration, and other automated tests.

**Status:** Under Evaluation.

The selected testing framework must support:
- TypeScript.
- Backend testing.
- Frontend component testing.
- Mocking and test isolation.
- CI execution.

Official documentation candidates:

Vitest:
https://vitest.dev/guide/

Node.js test runner:
https://nodejs.org/api/test.html

The final framework should be selected based on the actual project requirements, not by introducing multiple overlapping testing frameworks unnecessarily.

### 12.5 End-to-End Testing

**Purpose:** Verify complete user journeys.

**Status:** Under Evaluation.

Potential candidate:

Playwright:
https://playwright.dev/docs/intro

Evaluation requirements:
- Browser automation.
- Authentication flow testing.
- Repository workflow testing.
- Chat and citation navigation testing.
- CI compatibility.

---

## 13. Logging and Monitoring

### 13.1 Structured Logging

**Purpose:** Consistent operational diagnostics.

**Status:** Required capability; exact implementation package to be selected.

Potential candidate:

Pino:
https://getpino.io/

Requirements:
- Structured logs.
- Request identifiers.
- Job identifiers.
- Error categories.
- Operation durations.
- Safe diagnostic context.
- No secrets or unnecessary sensitive content.

### 13.2 Error Tracking

**Purpose:** Identify production exceptions and application failures.

**Status:** Under Evaluation.

Potential candidate:

Sentry:
https://docs.sentry.io/

Requirements:
- Capture actionable exceptions.
- Avoid sending secrets or sensitive repository content.
- Separate environments.
- Review data retention and pricing.
- Avoid unnecessary performance overhead.

### 13.3 Metrics and Monitoring

**Purpose:** Track application performance and reliability.

**Status:** Required capability; exact tools to be selected.

Potential capabilities:
- API latency.
- Error rates.
- Job duration.
- Ingestion failures.
- Model usage.
- External service failures.
- Database health.

Prometheus and Grafana may be considered where their operational cost and complexity are justified.

Official documentation:

https://prometheus.io/docs/introduction/overview/

https://grafana.com/docs/

Do not introduce a complex monitoring stack before the MVP needs it.

---

## 14. Storage and Caching

### 14.1 Temporary File Storage

**Purpose:** Temporary repository download and processing.

**Status:** Selected initial approach.

Requirements:
- Use bounded temporary storage.
- Clean up temporary files.
- Avoid persistent storage assumptions.
- Prevent unsafe paths.
- Enforce disk and file-size limits.

### 14.2 Object Storage

**Purpose:** Durable storage if future processing requirements justify it.

**Status:** Deferred / Under Evaluation.

Potential candidate:

Cloudflare R2:
https://developers.cloudflare.com/r2/

Object storage must only be introduced when durable persistence is genuinely required.

### 14.3 Redis Caching

**Purpose:** Optional caching or queue infrastructure.

**Status:** Deferred unless justified.

Do not introduce Redis merely for architectural appearance.

Evaluate actual caching needs and operational cost before adoption.

---

## 15. Deployment and Infrastructure

### 15.1 Frontend Hosting

**Purpose:** Deploy the React/Vite frontend.

**Status:** Preferred candidate.

Cloudflare Pages:
https://developers.cloudflare.com/pages/

Requirements:
- Secure environment configuration.
- Production build validation.
- Correct API configuration.
- HTTPS.
- Appropriate deployment workflow.

### 15.2 Backend Hosting

**Purpose:** Host the Fastify API.

**Status:** Under Evaluation.

Potential candidates:

Railway:
https://docs.railway.com/

Render:
https://render.com/docs

AWS remains an alternative where operational requirements justify it.

Evaluation criteria:
- Cost.
- Deployment workflow.
- Runtime reliability.
- Background worker compatibility.
- Environment configuration.
- Scaling behavior.
- Database connectivity.
- Observability.

### 15.3 Background Worker Deployment

**Purpose:** Execute repository ingestion and indexing jobs.

**Status:** Selected architectural requirement.

The worker must be independently deployable.

It may initially share a machine or hosting environment with the backend, but its execution and deployment responsibilities must remain separate.

### 15.4 Remote Container Builds

**Purpose:** Build Docker images without requiring Docker locally.

**Status:** Selected constraint and deployment direction.

The developer's local machine cannot reliably run Docker or WSL.

Therefore:
- Do not require local Docker for normal development.
- Use remote cloud builds or GitHub Actions for image construction.
- Run containers in the selected deployment environment where required.
- Compare build cost and operational complexity before finalizing the build provider.

### 15.5 GitHub Actions

**Purpose:** CI workflows and potential remote image builds.

**Status:** Candidate / Preferred CI direction.

Official documentation:

https://docs.github.com/en/actions

Potential responsibilities:
- Linting.
- Formatting checks.
- Type checking.
- Automated tests.
- Production builds.
- Remote container image builds.
- Deployment workflows.

Do not expose deployment credentials in workflow logs or source code.

---

## 16. Libraries and Tools Not Required by Default

The following technologies should not be introduced without a clear requirement:

| Technology | Current position | Reason |
|---|---|---|
| Microservices frameworks | Deferred | Modular monolith is sufficient for the MVP. |
| Kubernetes | Deferred | Unnecessary operational complexity initially. |
| Redis | Conditional | Introduce only when queue or caching requirements justify it. |
| Object storage | Conditional | Temporary worker storage is the initial approach. |
| Separate vector database | Not planned | PostgreSQL and pgvector provide the approved initial direction. |
| Multi-agent frameworks | Deferred | Not required for initial repository understanding. |
| Autonomous coding tools | Out of scope | The application is read-only for repository analysis. |
| Complex distributed caching | Deferred | No demonstrated need initially. |
| Dedicated reranking service | Deferred | Evaluate baseline retrieval first. |
| Private repository integrations | Out of scope for MVP | Public repositories only. |

These are not permanent prohibitions. Any future introduction must be justified and approved.

---

## 17. Version Management Policy

No exact package versions are frozen by this document.

Before implementation:

- Verify current stable versions.
- Prefer supported releases.
- Review breaking changes.
- Check runtime compatibility.
- Check provider compatibility.
- Commit dependency lockfiles.
- Avoid unnecessary version ranges for critical production dependencies.
- Review major upgrades before adoption.
- Test dependency upgrades before merging.

Do not blindly install the latest version without checking compatibility.

Version selection must be based on official documentation and verified package release information.

---

## 18. Dependency Selection and Approval Rules

Before introducing a new dependency:

1. Identify the problem it solves.
2. Check whether existing project dependencies already solve it.
3. Review official documentation.
4. Verify maintenance and release activity.
5. Review security advisories.
6. Evaluate license compatibility.
7. Evaluate bundle size or runtime overhead where relevant.
8. Evaluate operational complexity.
9. Evaluate pricing and free-tier limitations.
10. Confirm compatibility with the approved architecture.

Significant dependencies and services must be reviewed before adoption.

Do not introduce multiple libraries that solve the same problem without a demonstrated need.

---

## 19. Documentation Verification Rules

Official documentation must be treated as the primary source for third-party capabilities.

Before using a library or provider:

- Verify its current API.
- Verify supported runtime versions.
- Verify configuration requirements.
- Verify security recommendations.
- Verify limitations and pricing.
- Verify migration and upgrade procedures.
- Confirm that the intended feature is actually supported.

Do not rely solely on outdated tutorials, blog posts, or AI-generated examples.

If documentation does not establish a capability, treat it as unverified until confirmed.

---

## 20. Project-Specific Constraints

All technology decisions must respect these project constraints:

1. The architecture remains a modular monolith.
2. Frontend, backend, and ingestion worker remain independently deployable.
3. PostgreSQL and pgvector are the approved initial database and vector-search direction.
4. Public GitHub repositories only for the MVP.
5. Repository content is always untrusted.
6. Local development must not depend on Docker, WSL, or local containers.
7. Cloud-hosted development dependencies must be secured and appropriately isolated.
8. Docker images may be built remotely.
9. Avoid unnecessary managed services and recurring costs.
10. Prefer free development and low-cost production options where practical.
11. Provider choices must remain replaceable where doing so provides genuine value.
12. No unnecessary microservices, multi-agent systems, or infrastructure complexity.

---

## 21. Implementation Readiness

This document is one of the nine required planning documents.

Implementation must not begin until all nine documents have been finalized and explicitly approved:

1. `project-overview.md`
2. `architecture.md`
3. `build-plan.md`
4. `code-standards.md`
5. `library-docs.md`
6. `ui-tokens.md`
7. `ui-rules.md`
8. `ui-registry.md`
9. `progress-tracker.md`

Before implementation, the selected providers, package versions, and library integrations must be verified against current official documentation.

---

## 22. Final Principle

Use the smallest reliable technology stack that satisfies DevDocs AI's requirements.

Prefer well-maintained libraries, clear integration boundaries, secure defaults, and manageable operational complexity.

Do not select technologies because they are fashionable or appear sophisticated.

Every dependency must have a clear purpose, verified capabilities, and a justified place in the architecture.
