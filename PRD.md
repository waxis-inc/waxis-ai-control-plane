# PRD: Waxis Enterprise AI Control Plane

Document status: Draft v1.0  
Owner: Waxis Inc  
Research snapshot: 2026-06-16 EDT  
Repository: `waxis-ai-control-plane`

## 1. Problem Statement

Companies are adopting multiple AI models, agent frameworks, internal tools, and knowledge systems, but the production control layer is often fragmented. Individual teams hard-code provider SDKs, store model keys in application services, manually track token spend, and rely on inconsistent prompt, guardrail, evaluation, and audit practices.

The result is a risky AI estate:

- Model routing is brittle and tied to application code.
- API keys, rate limits, costs, and provider reliability are managed team by team.
- Security teams cannot consistently enforce prompt-injection mitigation, PII handling, access control, or output validation.
- Product teams cannot compare model quality, replay incidents, or measure latency and cost by workflow.
- Enterprise agents need standardized access to internal tools, but each integration is custom.

## 2. Solution Summary

Waxis Enterprise AI Control Plane is a production platform that sits between business applications and model/tool providers. It provides a unified gateway for model calls, a governance layer for policy and approvals, a connector registry for tools and data, and an observability and evaluation suite for continuous improvement.

The solution is not a generic chatbot. It is the foundation layer for all Waxis solutions and for client-owned AI systems that must be deployable, auditable, and maintainable.

## 3. Research-Backed Product Principles

1. Govern AI traffic at the gateway, not inside every application. Model selection, provider keys, quotas, logging, and failover should be centralized.
2. Treat agents as production workflows. Tool calls, approvals, retries, permissions, and handoffs must be observable and replayable.
3. Build for multi-model operations. Provider lock-in should be reduced through policy-based routing, model abstraction, and measurable quality/cost tradeoffs.
4. Use standards where possible. MCP should be supported for tool/data connectors, and telemetry should map cleanly to GenAI observability conventions.
5. Make risk management explicit. NIST AI RMF and OWASP LLM risks should translate into product controls, not just documentation.
6. Optimize cost and latency as first-class product outcomes. Prompt caching, context shaping, streaming, and fallback policy are product features.

## 4. Market And Technology Signals

- NIST released a Generative AI profile for the AI Risk Management Framework to help organizations identify and manage GenAI-specific risks.
- OWASP 2025 LLM guidance highlights prompt injection, insecure output handling, sensitive information disclosure, excessive agency, and model denial of service as design-time risks.
- MCP has become a standard way for AI applications to connect to external tools, data sources, and workflows.
- OpenAI Agents SDK supports hosted tools, file search, code execution, MCP, deferred tool loading, function tools, and tracing-oriented agent execution.
- Anthropic prompt caching and rate-limit guidance show that repeated prompts, tool definitions, and large context blocks should be optimized for cost and throughput.
- Amazon Bedrock Guardrails demonstrates enterprise demand for model-agnostic safety and privacy controls across applications.

## 5. Target Customers

### Primary

- Mid-market and enterprise B2B companies building more than one AI-enabled workflow.
- CTO, VP Engineering, Head of Data, Head of AI Platform, or Operations leadership.
- Regulated or compliance-aware teams that require logs, approvals, audit trails, and policy enforcement.

### Secondary

- SaaS companies adding AI features across multiple product modules.
- Internal platform teams standardizing LLM access for business departments.
- Agencies or system integrators that need a repeatable AI delivery foundation.

## 6. Personas

### AI Platform Owner

Needs model routing, provider abstraction, cost controls, service-level visibility, and release governance.

### Application Developer

Needs one stable API for model calls, tools, streaming, structured outputs, retries, and traces.

### Security And Compliance Reviewer

Needs audit logs, data boundaries, policy controls, PII handling, risk reports, and approval workflows.

### Business Workflow Owner

Needs confidence that AI agents can operate inside a real process with human checkpoints and measurable outcomes.

### Finance Or Operations Leader

Needs budget controls, spend allocation, trend visibility, and ROI by workflow.

## 7. Goals

1. Provide a unified, governed API for model calls across OpenAI, Anthropic, AWS Bedrock, Google, local models, and future providers.
2. Support policy-based routing by task, tenant, cost target, latency SLO, data classification, region, and fallback priority.
3. Centralize prompt/version management, model configuration, and release approvals.
4. Provide observable traces for model calls, tool calls, agent steps, errors, latency, token usage, and cost.
5. Enable guardrails for prompt injection, PII, harmful output, sensitive data, tool permissions, and insecure downstream actions.
6. Support evaluations, replay, regression testing, and approval gates before prompt/model/workflow changes go live.
7. Provide a tool connector registry with MCP-compatible connector support and permission-aware tool execution.

## 8. Non-Goals

- The product will not replace every client application UI.
- The product will not train foundation models in MVP.
- The product will not guarantee model factuality without domain context and evaluation data.
- The product will not become a generic API management platform for non-AI traffic.
- The product will not expose raw provider keys to application teams.

## 9. Product Scope

### MVP

- Unified model gateway for text, tool-calling, and structured-output workloads.
- Provider adapter layer for at least OpenAI, Anthropic, and AWS Bedrock.
- Workspace, environment, project, and API key management.
- Policy-based routing with fallback rules.
- Prompt registry with versioning and approval states.
- Request/response trace capture with redaction options.
- Usage and cost dashboard by workspace, project, provider, model, and workflow.
- Guardrail policy configuration for PII redaction, blocked topics, prompt injection indicators, and tool approval requirements.
- Evaluation dataset manager and replay runner.
- Admin UI and developer API docs.

### V1

- MCP connector registry and tool namespace management.
- Human approval queues for high-risk tool calls.
- Semantic caching and prompt-cache optimization suggestions.
- Multi-region routing and tenant-specific data boundary rules.
- SLO alerts, budget alerts, and incident replay.
- Advanced RBAC and audit export.
- Workflow-level quality scorecards.

### Future

- On-prem or VPC deployment package.
- Private model hosting integration.
- Automated model selection using live quality/cost benchmarks.
- Policy-as-code and Git-backed change control.
- Marketplace for Waxis-certified connectors and guardrail packs.

## 10. Core User Stories

1. As an AI platform owner, I want one gateway for all model calls, so that teams do not hard-code provider-specific integrations.
2. As an AI platform owner, I want to route requests by policy, so that high-risk workflows use approved models and low-risk workflows optimize for cost.
3. As a developer, I want a stable API for chat, structured outputs, streaming, and tool calls, so that I can build applications without provider lock-in.
4. As a developer, I want request traces with input, output, tools, latency, tokens, and errors, so that I can debug production AI behavior.
5. As a security reviewer, I want sensitive fields redacted in logs, so that observability does not leak confidential data.
6. As a compliance reviewer, I want immutable audit events for prompt, policy, routing, and key changes, so that production decisions are reviewable.
7. As a business owner, I want human approval for high-risk tool actions, so that agents cannot make irreversible changes without confirmation.
8. As a finance owner, I want spend by workflow and team, so that AI cost can be allocated and controlled.
9. As an operations owner, I want provider fallback rules, so that a provider outage does not break customer workflows.
10. As a product manager, I want to compare model quality on golden datasets, so that releases improve outcomes rather than only changing prompts.
11. As a security reviewer, I want prompt-injection and excessive-agency controls, so that agents cannot be manipulated into unsafe actions.
12. As a developer, I want MCP-compatible tool connectors, so that internal systems can be exposed through a standard integration pattern.
13. As an admin, I want environment-level configs, so that development, staging, and production policies are separated.
14. As an evaluator, I want to replay failed production examples, so that regressions can be fixed and verified.
15. As a platform owner, I want budget alerts and quota controls, so that runaway usage does not create surprise cost.

## 11. Functional Requirements

### 11.1 Workspace And Tenant Management

- FR-001: Support organizations, workspaces, projects, environments, users, groups, and service accounts.
- FR-002: Support role-based access for admin, developer, reviewer, finance, read-only, and service roles.
- FR-003: Support tenant-specific provider credentials, data-retention policies, and allowed model lists.
- FR-004: Provide audit logs for authentication, configuration, policy, prompt, key, and routing changes.

### 11.2 Model Gateway

- FR-010: Provide one gateway endpoint for model calls.
- FR-011: Support text generation, structured output, streaming, tool-calling, embeddings, and retrieval-augmented requests where provider adapters support them.
- FR-012: Support provider adapters for OpenAI, Anthropic, AWS Bedrock, and a generic OpenAI-compatible API.
- FR-013: Normalize model response metadata, including model, provider, latency, tokens, finish reason, tool calls, and error code.
- FR-014: Support retries, timeouts, circuit breakers, fallback provider selection, and streaming interruption handling.
- FR-015: Support request classification by workflow, use case, tenant, data class, user role, and cost center.

### 11.3 Routing Policy

- FR-020: Allow admins to create routing policies with conditions and ordered actions.
- FR-021: Support routing dimensions: model capability, cost ceiling, latency SLO, region, data classification, customer tier, provider health, and workflow risk level.
- FR-022: Allow traffic splitting for controlled experiments.
- FR-023: Support hard blocks for disallowed providers or models.
- FR-024: Log routing decision reasons for every request.

### 11.4 Prompt And Configuration Registry

- FR-030: Store prompt templates, system instructions, tool schemas, model settings, guardrail bindings, and output schemas.
- FR-031: Version every prompt and configuration artifact.
- FR-032: Support draft, review, approved, active, deprecated, and archived states.
- FR-033: Support environment promotion with approval gates.
- FR-034: Support test runs against evaluation datasets before activation.

### 11.5 Guardrails And Policy Enforcement

- FR-040: Support input and output guardrails for PII, secrets, harmful content, policy-prohibited content, prompt injection indicators, and malformed structured output.
- FR-041: Support tool-call policies by tool, user role, workflow, data class, and risk rating.
- FR-042: Support human approval before executing configured tool calls.
- FR-043: Support output validation against schemas and downstream action requirements.
- FR-044: Provide policy simulation mode for testing guardrails before enforcement.
- FR-045: Support policy outcomes: allow, warn, redact, request clarification, require approval, block, and escalate.

### 11.6 Tool Connector Registry

- FR-050: Register internal tools with name, description, schema, owner, environment, permissions, and risk rating.
- FR-051: Support MCP-compatible connectors for tools and data sources.
- FR-052: Support tool namespaces so agents can discover groups of related tools without loading every schema up front.
- FR-053: Track tool execution inputs, outputs, duration, errors, approvals, and side effects.
- FR-054: Support test mode and mock responses for tools.

### 11.7 Observability

- FR-060: Capture traces for model calls, agent steps, tool calls, retrieval operations, approvals, guardrail events, and final responses.
- FR-061: Track latency, token usage, cache hit rate, provider errors, retries, cost, and quality scores.
- FR-062: Provide dashboards by workspace, project, workflow, model, provider, tenant, and cost center.
- FR-063: Support log redaction and configurable retention.
- FR-064: Provide incident replay for selected traces.

### 11.8 Evaluation And Release Quality

- FR-070: Support golden datasets with inputs, expected behavior, rubrics, labels, and sensitive-data annotations.
- FR-071: Run evaluations across prompt versions, models, providers, and routing policies.
- FR-072: Support evaluator types: exact match, semantic similarity, structured schema validation, rubric-based LLM judge, tool-call correctness, latency, and cost.
- FR-073: Provide regression reports before promotion.
- FR-074: Allow production examples to be promoted into evaluation datasets after review.

### 11.9 Cost And Budget Management

- FR-080: Track cost by provider pricing table, model, tokens, request count, and tool use.
- FR-081: Support monthly budgets by workspace, project, tenant, and cost center.
- FR-082: Support soft alerts, hard quota blocks, and fallback-to-cheaper-model policies.
- FR-083: Provide prompt-cache and context-size recommendations.

## 12. Non-Functional Requirements

- NFR-001: Gateway p95 overhead should be less than 100 ms excluding provider latency for non-streaming calls in standard cloud deployment.
- NFR-002: Gateway should support horizontal scaling and stateless request workers.
- NFR-003: Trace storage must support configurable retention and redaction.
- NFR-004: Admin operations must be auditable.
- NFR-005: No provider secret should be exposed to client-side applications or normal developer logs.
- NFR-006: High-risk tool calls must be idempotency-aware where possible.
- NFR-007: The system must provide graceful degradation when an observability backend is unavailable.
- NFR-008: All API endpoints must support tenant-aware authorization.
- NFR-009: Production configuration changes must be reversible.

## 13. Data Model Concepts

- Organization
- Workspace
- Project
- Environment
- Provider Credential
- Model Adapter
- Routing Policy
- Prompt Version
- Guardrail Policy
- Tool Connector
- MCP Server Registration
- Request Trace
- Agent Run
- Tool Call
- Approval Event
- Evaluation Dataset
- Evaluation Run
- Budget
- Audit Event

## 14. Key Workflows

### Developer Integration

1. Developer creates a project.
2. Admin assigns allowed providers and models.
3. Developer creates an API key scoped to the project and environment.
4. Developer calls the Waxis gateway instead of provider SDKs directly.
5. Request is routed, guarded, traced, and billed.

### Prompt Release

1. Product owner drafts a prompt version.
2. Evaluator runs golden dataset checks.
3. Reviewer approves the prompt for staging.
4. Staging traffic validates behavior.
5. Admin promotes to production with rollback available.

### High-Risk Tool Call

1. Agent proposes a tool action.
2. Guardrail checks tool risk, user role, and workflow state.
3. Approval queue receives a structured action summary.
4. Human approves, rejects, or edits.
5. Tool executes and the trace records the approval and outcome.

## 15. Analytics And KPIs

### Product Adoption

- Number of active projects
- Number of gateway requests per week
- Number of registered workflows
- Provider coverage by client
- Percentage of AI traffic routed through the control plane

### Reliability

- Gateway availability
- Provider error rate
- Fallback success rate
- p50, p95, p99 latency
- Streaming interruption rate

### Governance

- Percentage of prompts under version control
- Percentage of production changes with evaluation runs
- Number of blocked or escalated guardrail events
- Approval SLA for high-risk tool calls
- Audit export completeness

### Cost

- Cost per workflow outcome
- Token usage by project
- Cache hit rate
- Budget variance
- Cost avoided through fallback or caching policy

### Quality

- Evaluation pass rate
- Regression count per release
- Human override rate
- Tool-call correctness rate
- Incident replay closure time

## 16. Security And Governance Requirements

- All provider keys must be encrypted at rest.
- Support least-privilege service accounts.
- Support private deployment mode for clients with strict data boundaries.
- Support redaction policies for PII, secrets, and configured sensitive fields.
- Support configurable data retention by tenant.
- Every production prompt, policy, model, tool, and routing change must create an audit event.
- Guardrails must cover input, output, retrieval context, and tool invocation.
- Tool calls that can change customer, financial, inventory, or external system state must support approval gates.
- Product security reviews must include OWASP LLM risks.
- AI risk review templates must map to NIST AI RMF categories.

## 17. UX Requirements

### Admin Console

- Workspace overview with traffic, spend, quality, and risk status.
- Provider credential manager.
- Routing policy editor with simulation.
- Prompt registry with diff, version history, and approval state.
- Guardrail policy builder.
- Tool connector registry.
- Trace explorer.
- Evaluation runner.
- Budget dashboard.

### Developer Experience

- Quickstart guide.
- API key generation.
- SDK examples.
- Request explorer.
- Provider capability matrix.
- Clear error messages for policy blocks, provider errors, schema failures, and quota limits.

### Reviewer Experience

- Approval queue with action summary, risk reason, source context, and previous related actions.
- Diff view for prompt and policy changes.
- Evaluation report view with pass/fail, examples, and regression details.

## 18. Implementation Decisions

- Build the gateway as a provider-agnostic orchestration service with adapters.
- Keep provider-specific details behind normalized request and response contracts.
- Store prompts, policies, and tool definitions as versioned artifacts.
- Make evaluation a release prerequisite for production prompt and routing changes.
- Use MCP-compatible connector patterns for future portability.
- Separate traces from raw logs so redaction and retention are controllable.
- Treat every tool call as a security-sensitive action with risk metadata.
- Prefer explicit workflow IDs over free-form tags for cost and quality reporting.

## 19. Testing Decisions

- Test behavior at the gateway contract level, not provider implementation details.
- Use provider mocks for routing, retry, fallback, streaming, and error behavior.
- Maintain golden datasets for each client workflow.
- Run policy simulation tests for guardrail outcomes.
- Test audit log creation for every sensitive admin action.
- Test RBAC and tenant isolation with negative cases.
- Load test routing, tracing, and cost aggregation paths separately.
- Run chaos tests for provider outage and observability backend failure.

## 20. Rollout Plan

### Phase 0: Design Partner Setup

- Select 1-2 client workflows.
- Identify providers, data classes, and high-risk actions.
- Define first golden datasets.
- Configure budgets and approval owners.

### Phase 1: MVP Deployment

- Route selected workflows through the gateway.
- Enable tracing, cost dashboard, prompt registry, and base guardrails.
- Run weekly quality and cost reviews.

### Phase 2: Governance Expansion

- Add evaluation gates.
- Add fallback and budget policies.
- Add high-risk tool approval queues.
- Add more teams and workflows.

### Phase 3: Platform Standardization

- Make control plane mandatory for production AI traffic.
- Expand MCP connectors.
- Add policy-as-code and private deployment options.

## 21. Risks And Mitigations

| Risk | Mitigation |
| --- | --- |
| Gateway becomes a bottleneck | Keep workers stateless, use async provider calls, add health-based routing and circuit breakers |
| Logs expose sensitive data | Default redaction, configurable retention, field-level masking, and restricted trace access |
| Teams bypass the platform | Provide simple SDKs, strong docs, and cost/quality dashboards that teams need |
| Guardrails block valid work | Support policy simulation, warning mode, reviewer override, and feedback loops |
| Model abstraction hides useful provider features | Expose capability flags and provider-specific advanced options behind controlled configuration |
| Evaluation quality is weak | Require workflow-specific rubrics and production-example promotion |

## 22. Open Questions

1. Should MVP support self-hosted local models or only OpenAI-compatible endpoints for local/private models?
2. Which clients require private deployment from day one?
3. Should cost calculations use live provider pricing sync or manually approved price tables?
4. What is the minimum audit export format required for compliance buyers?
5. Which MCP servers should Waxis certify first?
6. How much raw prompt/response content should be retained by default?

## 23. Acceptance Criteria

- A developer can make model calls through one Waxis endpoint without direct provider credentials.
- An admin can configure provider credentials, allowed models, routing policies, guardrails, and budgets.
- Every request has a trace with model, provider, routing decision, latency, token usage, cost, and guardrail outcome.
- A prompt change cannot be promoted to production without an approval event.
- A high-risk tool call can require human approval before execution.
- A provider outage can trigger configured fallback routing.
- An evaluator can run a golden dataset against two prompt/model variants and compare results.
- A finance user can view cost by workspace, project, provider, model, and workflow.

## 24. Research References

- NIST AI Risk Management Framework and Generative AI Profile: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLMs and Gen AI Apps 2025: https://genai.owasp.org/llm-top-10/
- Model Context Protocol introduction: https://modelcontextprotocol.io/docs/getting-started/intro
- OpenAI Agents SDK tools: https://openai.github.io/openai-agents-python/tools/
- OpenAI File Search and vector stores: https://developers.openai.com/api/docs/guides/tools-file-search
- Anthropic Claude prompt caching: https://platform.claude.com/docs/en/build-with-claude/prompt-caching
- Anthropic Claude rate limits and cache guidance: https://platform.claude.com/docs/en/api/rate-limits
- Amazon Bedrock Guardrails: https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html
- Amazon Bedrock Agents: https://docs.aws.amazon.com/bedrock/latest/userguide/agents-how.html

