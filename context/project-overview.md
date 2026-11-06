# DevDocs AI --- Project Overview

**Document:** `project-overview.md`\
**Status:** Draft generated following user approval\
**Document purpose:** Define what DevDocs AI is intended to do, who it
serves, its user-facing workflows, MVP scope, requirements, and
acceptance criteria.\
**Scope boundary:** This document defines product behavior and
high-level responsibilities. Detailed technology selection, database
schema, API contracts, folder structure, and implementation sequencing
belong in `architecture.md` and `build-plan.md`.

------------------------------------------------------------------------

## 1. Project Overview

DevDocs AI is an AI-powered codebase understanding platform that helps
developers explore unfamiliar public GitHub repositories, understand
architecture and implementation, trace behavior across files, and
investigate potential bugs.

Users can add a public repository, review its details and indexing
constraints, confirm indexing, and then ask repository-specific
questions through a conversational interface. The system retrieves
relevant repository evidence and uses it to produce explanations with
source citations that users can inspect.

### Core principle

**Evidence before assertion.** Repository-specific answers must be
grounded in retrieved repository evidence. The system must distinguish
repository facts from general technical explanations and clearly state
when available evidence is insufficient or conflicting. It must not
present unsupported guesses as verified implementation details.

### MVP direction

The MVP prioritizes a working end-to-end product with reliable
retrieval, verifiable citations, secure handling of untrusted repository
content, understandable failure states, repeatable evaluation, and a
deployed demonstration. The goal is not to maximize the number of
features at the expense of trustworthiness.

------------------------------------------------------------------------

## 2. Problem Statement

Understanding an unfamiliar codebase can require substantial manual
exploration. Important details may be distributed across source files,
documentation, configuration, and API specifications. Tracing a feature
or request through handlers, services, and data-access layers can be
difficult, and generic AI explanations may not accurately describe the
implementation in the repository being examined.

Developers need a way to ask questions about a repository and receive
useful explanations that are supported by evidence they can verify.

DevDocs AI aims to reduce the effort required to explore a supported
codebase while making the limits of its analysis visible.

------------------------------------------------------------------------

## 3. Intended Users

The MVP is designed for the following user groups:

  -----------------------------------------------------------------------
  User group                          Primary need
  ----------------------------------- -----------------------------------
  Individual developers               Explore unfamiliar repositories,
                                      understand implementation details,
                                      and investigate potential bugs

  Students and open-source            Learn how real projects are
  contributors                        structured and how features work
                                      across files

  Engineering teams                   Reduce the time needed to
                                      understand existing codebases
  -----------------------------------------------------------------------

Individual developers are the initial product focus. Supporting the
other groups does not imply that team workspaces, shared conversations,
organization roles, or enterprise administration are included in the
MVP.

------------------------------------------------------------------------

## 4. User Roles and Permissions

### 4.1 Application role

The MVP has one application-level role: **authenticated user**.

There is no separate administrator, organization manager, or team role
in the MVP product scope.

### 4.2 User permissions

An authenticated user may:

-   Create and access their own account.
-   Search for public GitHub repositories and submit public repository
    URLs.
-   Create and manage their own repository records.
-   View indexing status, supported/skipped content information, and
    relevant errors for repositories they own.
-   Open repository details and start repository-scoped conversations.
-   View their own conversation history.
-   Retry recoverable indexing failures and request repository
    refresh/re-indexing.
-   Remove a repository and its associated indexed data and
    repository-specific conversation history.

### 4.3 Isolation requirements

-   Each user's repository records, indexing metadata, conversations,
    and other user-specific application records must remain isolated.
-   A repository being public on GitHub does not grant access to another
    user's application records or conversations.
-   Access control must be enforced at the trusted server or data-access
    boundary, not solely by hiding UI elements.
-   The MVP does not provide shared repository workspaces or shared
    conversation access.
-   Shared indexing across users is deferred. Repository records are
    treated as user-isolated even if two users add the same public
    GitHub repository.

### 4.4 Authentication and account recovery

Authentication is required to access persistent, user-specific
application data. The exact authentication provider and sign-in method
remain open for the architecture phase. Account recovery is included
only if supported by the selected authentication mechanism.

------------------------------------------------------------------------

## 5. Core User Journeys and Workflows

### 5.1 Discover and add a repository

1.  The user visits the landing page.
2.  The user registers or signs in.
3.  The user opens the dashboard.
4.  The user searches for a public GitHub repository or submits its URL.
5.  DevDocs AI validates that the target is a supported public GitHub
    repository and checks applicable constraints.
6.  The application displays repository details and relevant indexing
    limitations.
7.  The user explicitly confirms that they want to index the repository.
8.  The application creates the user's repository record and starts
    indexing.
9.  The user can inspect indexing status, available progress
    information, skipped/unsupported files, and relevant errors.
10. Once useful content is available, the user can open the repository
    overview and start a conversation.

Accepting an indexing request must not be presented as proof that
indexing succeeded. The UI must reflect the actual indexing state.

### 5.2 Understand a repository

1.  The user opens an indexed repository.
2.  The repository overview displays repository identity, indexed
    revision, indexing status, and available actions.
3.  The user opens repository chat.
4.  The user asks a question about the codebase.
5.  DevDocs AI retrieves relevant repository evidence.
6.  The system generates an explanation grounded in that evidence.
7.  The response provides source paths and line ranges, with clickable
    source links tied to the indexed revision where feasible.
8.  The user asks follow-up questions. Conversation context is retained,
    and relevant evidence is retrieved again for the new question.

### 5.3 Investigate a bug or trace implementation

1.  The user describes a bug, API, function, feature, or observed
    behavior.
2.  DevDocs AI searches for relevant source files, documentation, and
    other indexed evidence.
3.  The system explains the relevant implementation flow or identifies
    code locations that may help investigate the issue.
4.  Important repository-specific claims are supported by citations so
    the user can verify them.
5.  If evidence is insufficient or conflicting, the system says so and
    avoids inventing an implementation detail.

Bug investigation in the MVP means helping the user understand and
locate potential causes. It does not mean autonomously modifying code or
guaranteeing a diagnosis.

### 5.4 Recover from indexing failure

1.  The user opens repository details.
2.  The application presents an understandable failure reason when
    known.
3.  The user can inspect available diagnostics and skipped-file
    information.
4.  The user can retry indexing when the failure is recoverable.
5.  Retries are bounded and controlled; infinite retry loops are not
    allowed.
6.  User-facing errors must not expose credentials, internal filesystem
    paths, or sensitive diagnostics.

### 5.5 Refresh a repository

1.  The user requests a refresh/re-index from repository details.
2.  DevDocs AI re-indexes the current default-branch revision.
3.  The system records the revision used for the new index.
4.  Existing conversation history is preserved.
5.  If a conversation's prior evidence refers to an older revision, that
    fact must be made clear. Citations must correspond to the revision
    of the evidence actually used for an answer; they must not silently
    point to different code.

The detailed revision storage, index replacement strategy, and exact UI
behavior are architecture decisions.

### 5.6 Remove a repository

1.  The user initiates repository removal from repository details.
2.  The application clearly explains that removal includes the
    repository record, associated indexed data, and repository-specific
    conversation history.
3.  The application starts the deletion and cleanup process.
4.  Cleanup failures must be surfaced rather than silently reported as
    complete.
5.  The system must not claim that deletion has fully completed until
    the relevant cleanup has been verified or any remaining limitation
    is explicitly disclosed.

------------------------------------------------------------------------

## 6. Application Pages and Routes

The paths below are proposed route names for product planning. They are
not yet final implementation contracts and may be refined in
`architecture.md`.

  ------------------------------------------------------------------------------------
  Page                    Proposed route                       Responsibility
  ----------------------- ------------------------------------ -----------------------
  Landing                 `/`                                  Explain the product,
                                                               its core capabilities
                                                               and limitations, and
                                                               provide
                                                               registration/sign-in
                                                               entry points

  Sign in                 `/login`                             Authenticate an
                                                               existing user

  Registration            `/signup`                            Allow a new user to
                                                               create an account

  Account recovery        To be determined                     Provide account
                                                               recovery only if
                                                               supported by the
                                                               selected authentication
                                                               method

  Dashboard               `/dashboard`                         Show the current user's
                                                               repositories, indexing
                                                               statuses, and entry
                                                               points for adding
                                                               repositories

  Add repository          `/repositories/new`                  Search public GitHub
                                                               repositories or accept
                                                               a repository URL;
                                                               display details and
                                                               constraints before
                                                               confirmation

  Repository overview     `/repositories/:repositoryId`        Show repository
                                                               details, indexed
                                                               revision, indexing
                                                               status, skipped
                                                               content, errors, and
                                                               available actions

  Repository chat         `/repositories/:repositoryId/chat`   Provide
                                                               repository-scoped chat,
                                                               follow-up questions,
                                                               cited answers, and
                                                               conversation history

  Not-found fallback      Application-level fallback           Explain that a
                                                               requested route or
                                                               resource could not be
                                                               found

  Error states            Within relevant pages, with a global Explain recoverable and
                          fallback where appropriate           unrecoverable errors
                                                               and provide appropriate
                                                               next actions
  ------------------------------------------------------------------------------------

There is **no dedicated user settings page** in the MVP.

Loading, empty, success, partial-indexing, and error states are part of
the relevant pages. They do not automatically require separate routes.

Navigation entry points and exit paths must remain consistent across
pages. Exact navigation and route-guard behavior will be specified
during architecture and UI planning.

------------------------------------------------------------------------

## 7. Functional Requirements

### FR-1: Authentication and account isolation

-   Require authentication before allowing access to persistent
    user-specific application data.
-   Support account registration and sign-in through the selected
    authentication mechanism.
-   Include account recovery only where the selected authentication
    mechanism supports it.
-   Enforce authorization at trusted server-side or data-access
    boundaries.
-   Restrict repository management and conversation access to authorized
    users.
-   Handle expired sessions, failed authentication, and unauthorized
    requests clearly.
-   Prevent one user from accessing another user's repository records,
    indexing metadata, or conversations through unauthorized requests.

### FR-2: Public repository discovery and management

-   Accept a public GitHub repository URL.
-   Provide public GitHub repository search.
-   Validate repository URLs, hosts, paths, redirects, and applicable
    access constraints.
-   Reject unsupported or unsafe destinations.
-   Display repository details and relevant limits before the user
    confirms indexing.
-   Support viewing repository status, retrying failed indexing,
    refreshing/re-indexing, and removing repositories.
-   Display relevant errors and skipped-file information where
    available.
-   Record the source revision used for indexing so that answers and
    citations can be associated with the correct snapshot.
-   Keep repository records isolated per user. Shared indexing is not
    part of the MVP.

### FR-3: Repository ingestion and indexing

-   Ingest supported source code, documentation, configuration files,
    and API specifications.
-   Provide a language-agnostic text/code retrieval baseline where
    practical, with enhanced structural understanding prioritized for
    JavaScript and TypeScript.
-   Define and enforce repository-size, file-size, content-type, and
    processing limits.
-   Exclude binaries, generated artifacts, irrelevant files, and
    detected secrets or sensitive content according to documented rules.
-   Report unsupported or skipped files and partial failures clearly.
-   Represent indexing states accurately; accepting a request is not
    equivalent to successful indexing.
-   Allow partial indexing with a clear warning when enough usable
    content has been processed to support meaningful repository
    exploration.
-   Do not represent a partially indexed repository as fully covered.
-   If too little usable content is available, report the indexing
    failure or insufficient usable content instead of presenting the
    repository as ready for reliable querying.
-   Support bounded retries and recoverable failure handling.

Exact file allowlists, resource limits, secret-detection rules, and the
threshold for "enough usable content" remain to be defined during
architecture and build planning.

### FR-4: Repository understanding

The product should support questions and explanations concerning:

-   Functions, classes, modules, and their responsibilities.
-   Architecture and component relationships.
-   API request flows through handlers, services, and data-access
    layers.
-   Authentication and authorization implementation.
-   Function usages, imports, and dependencies.
-   Configuration and setup instructions.
-   Repository summaries.
-   Relationships and comparisons across relevant files within the same
    repository.

These are intended capabilities, not a guarantee that every repository
or language can be analyzed perfectly. Structural analysis must remain
within the capabilities actually implemented and evaluated.

### FR-5: Evidence-grounded AI chat

-   Provide a conversational interface scoped to one repository per
    conversation.
-   Retrieve relevant repository evidence for repository-specific
    questions.
-   Include source paths and line ranges in citations.
-   Provide clickable source links tied to the correct indexed revision
    where feasible.
-   Preserve conversation context across follow-up questions while
    retrieving relevant evidence for the new question.
-   Distinguish repository facts from general technical explanations.
-   State when available evidence is insufficient or conflicting.
-   Avoid presenting unsupported guesses as verified repository facts.
-   Treat repository files and retrieved content as untrusted data, not
    as instructions that override application rules.
-   Ensure citations correspond to the evidence actually retrieved and
    used for the answer.

### FR-6: Conversation history

-   Persist conversation history for authenticated users.
-   Associate each conversation with the relevant user and repository.
-   Allow users to return to their prior conversations for a repository
    they are authorized to access.
-   Preserve history when a repository is refreshed, while identifying
    when prior evidence refers to an older indexed revision.
-   Delete repository-specific conversations when the associated
    repository is removed.
-   Keep conversations isolated between users.

Conversation sharing, exporting, renaming, and cross-repository
conversations are not included unless separately approved.

### FR-7: Indexing status and operational feedback

-   Display meaningful indexing states, including pending, in progress,
    successful, partially successful, and failed where applicable.
-   Display progress only when it can be reported accurately.
-   Distinguish unsupported repositories, inaccessible repositories,
    processing failures, and temporary external-service errors where
    possible.
-   Provide retry actions for recoverable indexing failures.
-   Show useful skipped-file and partial-coverage information.
-   Handle GitHub rate limits, timeouts, model-provider errors,
    storage/retrieval failures, and resource limits.
-   Avoid exposing secrets, internal paths, or sensitive repository
    content in user-facing errors and logs.
-   Track relevant usage, latency, failures, and cost indicators within
    the limits of the chosen stack.

### FR-8: Repository refresh and deletion

-   Refresh/re-index the current default-branch revision.
-   Associate each completed index with the revision actually processed.
-   Preserve conversation history across refreshes.
-   Make older-revision context clear where it matters to a conversation
    or citation.
-   Remove repository records, indexed data, and repository-specific
    conversation history when the user removes a repository.
-   Surface incomplete cleanup rather than claiming successful deletion
    without evidence.

### FR-9: Cost and usage safeguards

-   Prefer free tiers and cost-effective services where practical.
-   Review recurring costs before adopting paid services or
    dependencies.
-   Apply reasonable safeguards against unexpected model usage,
    processing volume, or hosting charges.
-   Measure cost-related indicators where feasible.
-   Do not promise zero operating cost or assume that a free tier will
    remain available indefinitely.

------------------------------------------------------------------------

## 8. High-Level Data Responsibilities

This section defines conceptual data categories and ownership
responsibilities only. It does not prescribe a final database schema or
technology.

  -----------------------------------------------------------------------
  Data category                       Responsibility
  ----------------------------------- -----------------------------------
  User identity                       Identify the authenticated user and
                                      support authorization

  Repository records                  Store repository identity,
                                      ownership, source information, and
                                      indexing status

  Indexing metadata                   Record source revision, processing
                                      state, errors, and
                                      supported/skipped content
                                      information

  Indexed content                     Store processed content and
                                      retrieval metadata needed to answer
                                      questions

  Retrieval index                     Find relevant passages or code
                                      segments for a user question

  Conversations                       Persist user questions, generated
                                      answers, citations, and
                                      conversation context

  Operational metadata                Measure processing duration,
                                      failures, usage, and costs without
                                      unnecessarily recording sensitive
                                      content
  -----------------------------------------------------------------------

### Data invariants

-   User-specific records must remain isolated by authorization.
-   A citation must not be fabricated; it must point to the evidence
    actually used.
-   Source revision information must be retained sufficiently to
    associate evidence and citations with the indexed snapshot.
-   Repository refresh must not silently make older conversation
    evidence appear to describe the latest source revision.
-   Repository deletion must trigger cleanup of dependent indexed
    content and repository-specific conversation history.
-   Secrets and sensitive content must not be indiscriminately indexed,
    exposed in generated answers, or included in logs.
-   Indexing and deletion states must reflect actual processing
    outcomes.

The detailed schema, storage systems, indexes, retention implementation,
and authorization policies belong in `architecture.md`.

------------------------------------------------------------------------

## 9. Security, Privacy, and Reliability Requirements

The following are mandatory product-level requirements for the MVP.
Concrete implementation controls and verification procedures will be
defined in the architecture and code-standards documents.

1.  **Repository URL safety:** Validate repository destinations and
    handle redirects and outbound network access safely to prevent SSRF
    and unsafe URL handling.
2.  **Untrusted repository content:** Treat source files, documentation,
    comments, and other repository data as untrusted. Defend against
    prompt injection embedded in retrieved content.
3.  **Secret handling:** Exclude detected secrets and sensitive files
    from indexing where possible. Avoid exposing credentials or
    sensitive content in logs, errors, and answers.
4.  **Authentication and authorization:** Require authentication for
    persistent user-specific data and enforce ownership checks at
    trusted boundaries.
5.  **Cross-user isolation:** Prevent unauthorized access to another
    user's repository records, indexed content, and conversations.
6.  **Input validation:** Validate and normalize inputs at appropriate
    trust boundaries.
7.  **Abuse prevention:** Apply rate limits, resource limits, timeouts,
    and bounded retries.
8.  **External dependency handling:** Handle GitHub, model-provider,
    storage, and retrieval failures and limits.
9.  **Cost controls:** Establish usage safeguards and monitor inference
    and hosting costs where feasible.
10. **Data deletion:** Propagate repository removal to dependent data
    and surface incomplete cleanup.
11. **Safe diagnostics:** Do not expose credentials, internal filesystem
    paths, or sensitive repository content through user-facing errors or
    logs.
12. **Verification:** Include appropriate functional, integration, and
    security tests. Document any safeguard that cannot be fully
    guaranteed.

------------------------------------------------------------------------

## 10. Explicit MVP Scope

### 10.1 In scope

-   Public GitHub repositories only.
-   Account-based access.
-   Public repository search and URL submission.
-   Repository detail preview and user confirmation before indexing.
-   Source-code and documentation ingestion within defined limits.
-   Partial indexing when enough content is usable, with explicit
    warnings about incomplete coverage.
-   Repository overview and indexing status.
-   Retrieval-augmented, evidence-grounded repository chat.
-   Source citations with paths and line ranges, plus source links tied
    to the indexed revision where feasible.
-   Follow-up questions with retained conversation context and
    appropriate new retrieval.
-   Persistent repository records and conversation history.
-   Repository refresh/re-indexing and bounded retries.
-   Repository removal with dependent-data cleanup.
-   Clear failure feedback and basic operational visibility.
-   Security safeguards, core tests, repeatable evaluation, cost
    awareness, and a deployed demo.

### 10.2 Out of scope for the MVP

-   Private GitHub repositories.
-   Autonomous code editing, commits, and pull-request creation.
-   Multi-agent orchestration.
-   Training a foundation model.
-   Team workspaces, enterprise administration, and organization-level
    roles.
-   Cross-repository reasoning.
-   IDE extensions.
-   Subscriptions and billing.
-   Shared indexing across users.
-   Dedicated user settings page.

These items may be reconsidered after the MVP is evaluated. Their
exclusion is not a commitment to implement them later.

------------------------------------------------------------------------

## 11. Acceptance Criteria and Measurable Success

The MVP must not be declared complete merely because the UI loads or the
model produces plausible text. Completion requires evidence that the
core user journeys and safeguards work.

  -----------------------------------------------------------------------
  Area                                Acceptance criterion
  ----------------------------------- -----------------------------------
  Repository onboarding               A supported public repository can
                                      be submitted, reviewed, confirmed,
                                      indexed, and opened

  Repository safety                   Unsafe or unsupported repository
                                      destinations are rejected according
                                      to the documented rules

  Indexing status                     Success, partial success, and
                                      failure states reflect actual
                                      outcomes

  Partial indexing                    When enough content is usable, the
                                      user can query it and receives a
                                      clear warning about incomplete
                                      coverage

  Insufficient usable content         The application does not present a
                                      repository as ready when too little
                                      usable content exists

  Repository understanding            Representative questions about
                                      supported codebases produce useful
                                      explanations grounded in retrieved
                                      evidence

  Citation correctness                Citations refer to relevant source
                                      content and the correct indexed
                                      revision where supported

  Insufficient evidence               The system acknowledges when
                                      repository evidence cannot support
                                      an answer

  Conversation context                Follow-up questions retain context
                                      and retrieve evidence appropriate
                                      to the new question

  Refresh behavior                    Refresh re-indexes the current
                                      default-branch revision, records
                                      the revision used, and preserves
                                      conversation history with
                                      older-revision context made clear

  Authorization                       Core authentication, ownership, and
                                      cross-user isolation cases have
                                      documented tests

  Security                            URL safety, untrusted-content
                                      handling, resource limits, and
                                      other applicable security controls
                                      have documented tests

  Failure recovery                    Recoverable indexing failures can
                                      be retried without unbounded retry
                                      loops

  Data lifecycle                      Repository removal triggers cleanup
                                      of the repository record, indexed
                                      data, and associated conversation
                                      history; incomplete cleanup is
                                      surfaced

  Evaluation                          A repeatable evaluation dataset and
                                      measurement procedure exist

  Deployment                          A working demo is deployed with
                                      safeguards against unexpected usage
                                      and cost

  Observability                       Relevant latency, failures, usage,
                                      and cost indicators can be
                                      inspected
  -----------------------------------------------------------------------

### 11.1 Baseline and numerical thresholds

Numerical thresholds for retrieval quality, citation correctness,
latency, reliability, and cost must be established from an initial
baseline and documented before the team claims the system meets them.

The baseline must be measured using a repeatable evaluation procedure
rather than guessed. The exact metrics, dataset, sample size, and
thresholds will be determined during the build-planning and evaluation
design work.

------------------------------------------------------------------------

## 12. Assumptions, Constraints, Dependencies, and Open Decisions

### 12.1 Confirmed decisions

-   The MVP will prioritize all major core capabilities while
    maintaining an incremental delivery approach.
-   Public GitHub repositories are the initial source scope.
-   Authentication is required.
-   Repository records, indexing metadata, and conversation history are
    persistent.
-   Users must confirm indexing after reviewing repository details and
    constraints.
-   The product has one application-level user role.
-   Each user's repository records and conversations remain isolated.
-   Shared indexing across users is deferred.
-   Each conversation is scoped to one repository.
-   The interface includes landing, authentication, dashboard,
    repository addition, repository overview, repository chat, and
    not-found/error states.
-   There is no dedicated user settings page.
-   Account recovery is included only if supported by the selected
    authentication mechanism.
-   Partial indexing is allowed when enough content is usable, with a
    clear warning about incomplete coverage.
-   Repository refresh re-indexes the current default-branch revision,
    preserves conversation history, and identifies older evidence where
    relevant.
-   Repository deletion includes associated indexed data and
    repository-specific conversation history.
-   Cost-conscious service selection and safeguards are required.
-   A deployed portfolio demonstration is an intended delivery outcome.
-   Delivery balances a working MVP with reliable retrieval, citations,
    security, and testing.
-   Development-environment and tooling restrictions will be specified
    separately.

### 12.2 Provisional recommendations

The following are working recommendations, not irreversible architecture
decisions:

-   Prioritize JavaScript/TypeScript structural understanding while
    maintaining a language-agnostic text/code retrieval baseline where
    practical.
-   Use the proposed route names as a starting point and finalize them
    during architecture and UI planning.
-   Keep repository records user-isolated even when repositories are
    identical and public.
-   Prefer useful partial indexing with clear coverage warnings rather
    than rejecting an otherwise usable repository because a limited
    subset of files failed.
-   Prioritize measurable trustworthiness over adding optional features.

### 12.3 Unresolved decisions

These decisions remain open and must not be silently invented by an
implementation agent:

-   Authentication provider and exact sign-in mechanism.
-   Exact repository, file-size, content, and processing limits.
-   Exact supported file extensions and secret-detection policy.
-   Precise threshold for determining whether enough content is usable
    for partial indexing.
-   Detailed revision tracking and index replacement behavior.
-   Development environment and tooling restrictions.
-   Exact LLM provider, embedding/retrieval implementation, database,
    storage, and hosting choices.
-   Exact evaluation dataset, metrics, and numerical thresholds.

Some of these decisions belong primarily in `architecture.md`,
`code-standards.md`, `library-docs.md`, or `build-plan.md`; they are
recorded here to make dependencies visible.

### 12.4 Explicitly deferred decisions and features

-   Private repository access.
-   Cross-repository reasoning.
-   Team and enterprise features.
-   Autonomous code modifications.
-   IDE integrations.
-   Billing and subscriptions.
-   Multi-agent workflows.
-   Shared indexing across users.

Deferred features must not be implemented unless the scope is explicitly
revised and approved.

------------------------------------------------------------------------

## 13. Relationship to Other Project Documents

This document defines **what** DevDocs AI should do and the
product-level constraints that must be preserved.

-   `architecture.md` will define how the approved requirements map to
    system components, technology choices, data flows, security
    boundaries, schemas, and API contracts.
-   `build-plan.md` will convert approved requirements and architecture
    into small, ordered, verifiable implementation phases.
-   `code-standards.md` will define enforceable coding, validation,
    security, testing, and quality rules.
-   `library-docs.md` will define how third-party capabilities and APIs
    are verified against authoritative documentation.
-   `ui-tokens.md` will define design tokens and visual foundations.
-   `ui-rules.md` will define reusable UI behavior and consistency
    rules.
-   `ui-registry.md` will track actual, verified reusable UI components
    rather than imagined or planned components.
-   `progress-tracker.md` will track approved tasks and verified
    progress.

If a later document reveals a conflict with this product scope, the
conflict must be surfaced and resolved explicitly. An implementation
agent must not silently change the requirements.

------------------------------------------------------------------------

## 14. Change Control

-   Requirements must be treated as confirmed only when explicitly
    agreed upon.
-   Provisional recommendations must not be represented as approved
    decisions.
-   Unresolved decisions must remain visible until resolved.
-   Deferred features must remain out of scope unless the scope is
    explicitly revised.
-   Changes affecting user journeys, page scope, permissions, data
    lifecycle, or acceptance criteria must be reviewed for their impact
    on the other project documents.
-   Significant scope changes must be explained and approved before
    implementation.
-   This document should be updated when approved product-level
    decisions change, rather than allowing conflicting instructions to
    accumulate across project documents.

------------------------------------------------------------------------

**End of document.**
