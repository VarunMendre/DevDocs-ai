# DevDocs AI — Build Plan

## 1. Purpose

This document defines the implementation roadmap for DevDocs AI.

It divides development into manageable, outcome-focused phases. Each phase establishes a meaningful part of the application before the project moves to the next stage.

The build plan is intended to:
- Provide a clear implementation sequence.
- Keep development tasks manageable and understandable.
- Maintain consistency with the approved project overview and architecture.
- Define expected outcomes and completion criteria.
- Help track implementation progress without introducing unnecessary complexity.

### 1.1 Documentation-First Requirement

Implementation must not begin until all nine planning documents have been finalized and explicitly approved:

1. `project-overview.md`
2. `architecture.md`
3. `build-plan.md`
4. `code-standards.md`
5. `library-docs.md`
6. `ui-tokens.md`
7. `ui-rules.md`
8. `ui-registry.md`
9. `progress-tracker.md`

Once all nine documents are approved, implementation may begin by following this build plan.

### 1.2 Execution Rules

- Follow the phases in their defined order unless an approved dependency requires a change.
- Complete the intended outcome of the current phase before proceeding.
- Break tasks into smaller subtasks only when doing so improves implementation or verification.
- Avoid combining unrelated features into one task.
- Follow the approved architecture and coding standards.
- Do not introduce unapproved dependencies, services, or architectural changes.
- Run relevant checks and tests before marking work complete.
- Update `progress-tracker.md` based on actual implementation results.
- Record blockers, deferred tasks, and known limitations.
- Request approval before making significant architectural changes.

---

## 2. Implementation Phases

The project will be developed through six phases.

## Phase 1 — Project Foundation

### Objective

Establish the development environment, repository structure, and foundational application infrastructure.

### Tasks

- Set up the monorepo and initial project structure.
- Configure the React, Vite, and TypeScript frontend.
- Configure the Node.js, Fastify, and TypeScript backend.
- Establish shared configuration and environment-variable handling.
- Configure code-quality and formatting tools.
- Establish database connectivity and migration infrastructure.
- Set up basic CI checks.
- Implement initial backend health endpoints.
- Create the initial frontend application shell.

### Dependencies

- Approved project overview.
- Approved architecture.
- Approved code standards and library choices before implementation begins.

### Expected Outcome

A working project foundation with independently runnable frontend and backend applications and an established database connection.

### Acceptance Criteria

- Frontend starts and builds successfully.
- Backend starts and responds to health checks.
- Frontend and backend can be developed and deployed independently.
- Database connectivity is verified.
- Environment configuration is handled consistently.
- Initial automated checks pass.

### Testing and Validation

- Verify frontend development and production builds.
- Verify backend startup and health endpoints.
- Verify database connectivity.
- Validate environment-variable handling.
- Run configured linting, formatting, and type checks.
- Verify CI executes the expected foundational checks.

---

## Phase 2 — Authentication and Core Data

### Objective

Establish secure user authentication, application data structures, and ownership boundaries.

### Tasks

- Integrate the selected authentication provider.
- Implement session verification.
- Protect authenticated backend routes.
- Establish initial database schemas and migrations.
- Implement user ownership and data-isolation rules.
- Build the basic authentication screens.
- Establish repository metadata management foundations.
- Handle authentication failures and expired sessions.

### Dependencies

- Phase 1 completed.
- Authentication and database provider decisions finalized.
- Initial database connectivity and migration infrastructure available.

### Expected Outcome

Users can authenticate and access protected application functionality. Application data is scoped to the authenticated user.

### Acceptance Criteria

- Users can complete the supported authentication flow.
- Protected endpoints reject unauthenticated requests.
- Session verification works correctly.
- User-owned resources cannot be accessed by other users.
- Database migrations can be applied consistently.
- Authentication errors are handled clearly.

### Testing and Validation

- Test successful and unsuccessful authentication.
- Test session expiration and invalid sessions.
- Test protected API routes.
- Test cross-user resource access attempts.
- Test database migrations.
- Verify authorization checks are enforced on the backend.

---

## Phase 3 — Repository Management and Ingestion

### Objective

Allow authenticated users to submit public GitHub repositories and process their source code for later retrieval.

### Tasks

- Implement GitHub repository URL validation.
- Retrieve repository metadata.
- Build the repository submission and confirmation workflow.
- Retrieve repository source pinned to a specific commit.
- Implement background ingestion jobs.
- Track ingestion status and progress.
- Apply repository, file-count, and file-size limits.
- Filter unsupported and irrelevant files.
- Implement secret detection safeguards.
- Extract usable source content.
- Add structural parsing for the selected initial languages.
- Generate and persist repository chunks.
- Record ingestion errors, skipped files, and partial results.

### Dependencies

- Phase 2 completed.
- Repository data structures and ownership rules available.
- GitHub retrieval strategy and ingestion boundaries established.

### Expected Outcome

Users can submit supported public repositories and receive meaningful ingestion results, including ready, partially ready, or failed states.

### Acceptance Criteria

- Invalid or unsupported repository URLs are rejected clearly.
- Repository metadata is retrieved and stored correctly.
- Source retrieval uses a pinned commit revision.
- File and repository limits are enforced.
- Unsupported files are excluded safely.
- Secret detection safeguards are applied.
- Ingestion jobs execute in the background.
- Job status and errors are available to the user.
- Partial ingestion is represented accurately.
- Retry behavior does not create inconsistent duplicate data.

### Testing and Validation

- Test valid and invalid repository URLs.
- Test repository metadata retrieval.
- Test source retrieval and revision consistency.
- Test file filtering and configured limits.
- Test secret detection.
- Test ingestion failures and retries.
- Test partially successful ingestion.
- Verify user ownership for repository and job operations.

---

## Phase 4 — Indexing and Retrieval

### Objective

Make ingested repository content searchable and retrieve relevant evidence for repository-related questions.

### Tasks

- Integrate the selected embedding provider.
- Generate embeddings for repository chunks.
- Persist embeddings and their associated metadata.
- Implement PostgreSQL full-text search.
- Implement vector similarity search using pgvector.
- Combine retrieval results using the approved ranking approach.
- Track embedding model and index versions.
- Establish repository-level summary generation.
- Preserve repository revision metadata.
- Create baseline retrieval evaluation tests.

### Dependencies

- Phase 3 completed.
- Repository chunks and metadata available.
- Embedding provider and model selected.
- Database vector-search infrastructure available.

### Expected Outcome

The application can retrieve relevant repository evidence using semantic and keyword-based search.

### Acceptance Criteria

- Repository chunks can be indexed successfully.
- Embeddings are associated with the correct repository and revision.
- Keyword and vector retrieval return relevant results.
- Combined retrieval produces usable ranked evidence.
- Retrieval results retain source and revision metadata.
- Index versions can be identified.
- Repository summaries are generated and stored where applicable.
- Retrieval failures are handled without presenting fabricated evidence.

### Testing and Validation

- Test embedding generation and persistence.
- Test keyword retrieval.
- Test vector retrieval.
- Test combined retrieval.
- Verify repository and user isolation.
- Verify source metadata and revision consistency.
- Evaluate retrieval relevance using representative repository questions.
- Record baseline evaluation results for future improvement.

---

## Phase 5 — AI Chat and Citations

### Objective

Allow users to ask questions about their repositories and receive useful answers grounded in retrieved source evidence.

### Tasks

- Integrate the selected chat model.
- Establish evidence-grounded prompt construction.
- Implement conversation and message management.
- Associate conversations with their repositories and owners.
- Implement fresh retrieval for follow-up questions.
- Add token-budgeted conversation context handling.
- Implement SSE-based response streaming.
- Support cooperative cancellation.
- Persist response and conversation states.
- Validate citations against stored repository evidence.
- Handle insufficient evidence explicitly.
- Handle incomplete and cancelled responses.
- Add answer-quality and citation evaluation tests.

### Dependencies

- Phase 4 completed.
- Chat model selected.
- Retrieval pipeline provides reliable evidence and metadata.
- Conversation and message data structures established.

### Expected Outcome

Users can have repository-specific conversations and receive streamed answers with verifiable citations.

### Acceptance Criteria

- Users can create and continue repository conversations.
- Conversations remain isolated by user and repository.
- Follow-up questions trigger fresh evidence retrieval.
- Responses stream correctly.
- Users can cancel ongoing responses.
- Citations reference valid stored evidence.
- Source paths, line ranges, and commit information are preserved where available.
- The application distinguishes repository-backed facts from general explanations.
- Insufficient evidence is communicated clearly.
- Incomplete responses are not represented as completed answers.

### Testing and Validation

- Test conversation creation and retrieval.
- Test repository and user isolation.
- Test follow-up retrieval.
- Test streaming and cancellation.
- Validate citation references and source metadata.
- Test insufficient-evidence scenarios.
- Test prompt-injection safeguards.
- Evaluate answer quality and citation correctness against representative questions.

---

## Phase 6 — Integration, Security and Release

### Objective

Complete the integrated user experience, validate the application's security and reliability, and prepare the MVP for public deployment.

### Tasks

- Complete dashboard workflows.
- Complete repository details and status interfaces.
- Complete repository chat interfaces.
- Implement loading, empty, error, and partial-indexing states.
- Improve navigation and overall application consistency.
- Complete end-to-end user journeys.
- Validate authorization and data isolation.
- Test prompt-injection defenses.
- Implement and validate API and per-user rate limits.
- Apply token and cost safeguards.
- Complete repository and account deletion workflows.
- Establish structured logging and operational metrics.
- Configure error tracking and health monitoring.
- Configure production cloud builds and deployment.
- Validate production environment configuration.
- Perform final retrieval and answer-quality evaluation.

### Dependencies

- Phases 1–5 completed.
- Core frontend and backend workflows integrated.
- Production hosting and build strategy selected.
- Security and operational requirements implemented or ready for validation.

### Expected Outcome

A deployed, usable MVP with complete core workflows, appropriate security safeguards, operational visibility, and documented limitations.

### Acceptance Criteria

- Core user journeys work end-to-end.
- Dashboard, repository details, and chat workflows are integrated.
- Loading, empty, error, and partial-indexing states are handled.
- User data and repository resources remain isolated.
- Repository deletion removes associated application data according to the approved deletion workflow.
- Rate limits and cost safeguards are active.
- Critical security checks pass.
- Logging, health checks, and error monitoring are operational.
- Production deployment succeeds.
- Final evaluation results and known limitations are documented.

### Testing and Validation

- Run end-to-end tests for core user journeys.
- Run authorization and cross-user isolation tests.
- Test prompt-injection and untrusted repository content scenarios.
- Test rate limits and budget safeguards.
- Test repository deletion and cleanup behavior.
- Validate production configuration.
- Verify deployment and health checks.
- Review retrieval quality, answer quality, and citation correctness.
- Record unresolved issues and known limitations before release.

---

## 3. Phase Execution Format

Every implementation phase should be managed using the following structure:

1. **Objective** — What the phase is intended to achieve.
2. **Tasks** — The meaningful implementation work required.
3. **Dependencies** — What must already be available.
4. **Expected Outcome** — What should exist when the phase is complete.
5. **Acceptance Criteria** — How completion will be verified.
6. **Testing and Validation** — Which checks demonstrate that the work behaves correctly.

Tasks should describe meaningful outcomes rather than individual code edits.

A task may be divided into smaller subtasks when its scope is too broad to implement, review, or verify reliably.

---

## 4. Task Management Rules

- Keep the active implementation scope manageable.
- Work on related tasks together when they share dependencies.
- Avoid starting multiple unrelated features simultaneously.
- Follow existing architecture and coding standards.
- Inspect existing implementation before modifying it.
- Do not assume an undocumented API, library method, or provider capability.
- Verify third-party functionality against reliable documentation.
- Do not mark a task complete based only on code being written.
- Record incomplete work and blockers honestly.
- Do not silently skip failed checks.
- Update relevant documentation when implementation decisions change.
- Request approval before introducing significant dependencies or changing architectural decisions.

If a task exposes a missing architectural decision, pause the affected work and request clarification or approval rather than making an unsupported assumption.

---

## 5. Phase Dependencies

The default implementation sequence is:

1. Project Foundation
2. Authentication and Core Data
3. Repository Management and Ingestion
4. Indexing and Retrieval
5. AI Chat and Citations
6. Integration, Security and Release

### Dependency Guidelines

- Each phase builds on the foundations established by earlier phases.
- Frontend work may progress alongside backend work when the required API contracts are sufficiently clear.
- UI implementation should follow the approved UI tokens, rules, and registry.
- Integration testing should begin as soon as stable interfaces are available.
- A phase should not be considered complete merely because its main feature works in isolation.
- Any deviation from the sequence must have a clear dependency-based reason and must not violate approved architecture decisions.

---

## 6. Definition of Done

A phase can be marked complete only when all applicable conditions are satisfied:

1. Planned tasks for the phase have been implemented.
2. Acceptance criteria have been met.
3. Relevant tests and validation checks have passed.
4. No unresolved critical security or data-isolation issue remains.
5. The expected phase outcome is demonstrably available.
6. Relevant documentation has been updated.
7. `progress-tracker.md` reflects the actual implementation status.
8. Known limitations, deferred work, and remaining risks are documented.
9. Actual test results and implementation outcomes have been reported.

A failed test, unverified behavior, or incomplete integration must not be reported as successful.

---

## 7. Scope and Change Management

This build plan provides the overall implementation roadmap. It does not permanently freeze every technical detail or prevent reasonable refinement.

However:

- Refining an implementation task does not automatically authorize an architectural change.
- New requirements must be evaluated against the approved project scope.
- Deferred features must remain deferred unless explicitly approved.
- Provider candidates must not be treated as selected providers until a decision is finalized.
- Significant changes to architecture, data ownership, security, or deployment must be reviewed and approved before implementation.

The six phases remain the primary development roadmap unless an approved change requires an update.

---

## 8. Final Execution Principle

Build DevDocs AI incrementally, with priority given to correctness, repository understanding, retrieval quality, verifiable citations, security, and a reliable end-to-end experience.

The goal is not to implement the maximum number of features as quickly as possible. The goal is to establish a working, understandable, maintainable, and demonstrably reliable MVP through controlled implementation phases