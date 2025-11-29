# A2A Protocol Specification Summary
**Source:** https://a2a-protocol.org/latest/specification/
**Version:** v1.0 (DRAFT)
**Date:** November 28, 2025

---

## 1. Core Characteristics of A2A

### 1.1. Guiding Principles (Section 1.2)
- **Simple:** Reuses existing standards (HTTP, JSON-RPC 2.0, SSE)
- **Enterprise Ready:** Authentication, authorization, security, privacy, tracing, monitoring
- **Async First:** Designed for long-running tasks and human-in-the-loop interactions
- **Modality Agnostic:** Supports text, audio/video, structured data, embedded UI
- **Opaque Execution:** Agents collaborate based on declared capabilities without sharing internal thoughts/plans

### 1.2. Key Goals (Section 1.1)
- **Interoperability:** Bridge disparate agentic systems
- **Collaboration:** Task delegation, context exchange, complex requests
- **Discovery:** Dynamic capability discovery
- **Flexibility:** Sync, streaming, async modes
- **Security:** Enterprise-suitable patterns, standard web security
- **Asynchronicity:** Native support for long-running tasks

---

## 2. Protocol Architecture

### 2.1. Three-Layer Architecture (Section 1.3)

**Layer 1: Canonical Data Model**
- Core data structures (Protocol Buffer definitions)
- Protocol-agnostic definitions
- Task, Message, AgentCard, Part, Artifact, Extension

**Layer 2: Abstract Operations**
- Fundamental capabilities independent of protocol
- Send Message, Stream Message, Get Task, List Tasks, etc.

**Layer 3: Protocol Bindings**
- JSON-RPC, gRPC, HTTP/REST
- Custom bindings possible

### 2.2. Normative Content (Section 1.4)
- **Single Source of Truth:** `spec/a2a.proto` file
- Guarantees protocol neutrality
- Deterministic evolution path

---

## 3. Core Operations (Section 3)

### 3.1. Message Operations
- **Send Message:** Initiate/continue task (sync or async)
- **Stream Message:** Real-time updates via SSE
- **Blocking vs Non-Blocking:** Configuration option for task execution

### 3.2. Task Management
- **Get Task:** Retrieve current state
- **List Tasks:** Filtering, pagination, context-based
- **Cancel Task:** Terminate processing
- **Subscribe to Task:** Real-time monitoring

### 3.3. Update Delivery Mechanisms (Section 3.5)
1. **Polling:** Simple, works everywhere, higher latency
2. **Streaming:** Real-time, low latency, persistent connection
3. **Push Notifications (WebHooks):** Async, no persistent connection required

---

## 4. Data Model (Section 4)

### 4.1. Core Objects

**Task (Section 4.1.1)**
- Fundamental unit of work
- Unique ID, contextId, status, artifacts, history
- Stateful lifecycle

**TaskState (Section 4.1.3)**
- SUBMITTED, WORKING, COMPLETED, FAILED, CANCELLED
- INPUT_REQUIRED (interrupted state)
- REJECTED, AUTH_REQUIRED

**Message (Section 4.1.4)**
- Communication unit between client/server
- Contains: messageId, contextId, taskId, role, parts, metadata, extensions

**Part (Section 4.1.6)**
- Text, File, or Data
- File supports URI or base64 bytes

**Artifact (Section 4.1.9)**
- Task outputs
- Contains parts, metadata, extensions

---

## 5. Multi-Turn Interactions (Section 3.4)

### 5.1. Context Identifier Semantics
- Server-generated or client-provided
- Logically groups related Tasks and Messages
- Maintains conversational continuity
- Enables context inheritance

### 5.2. Interaction Patterns
- **Input Required State:** Agent requests additional info
- **Follow-up Messages:** Continue/refine existing tasks
- **Task References:** `referenceTaskIds` field for explicit context linking

---

## 6. Agent Discovery (Section 8)

### 6.1. Agent Card
- Self-describing manifest
- Identity, capabilities, skills, endpoints, security requirements
- Discovery via:
  - Well-Known URI: `https://{domain}/.well-known/agent-card.json`
  - Registries/Catalogs
  - Direct configuration

### 6.2. Agent Card Contents
- **Protocol Version:** Supported A2A version
- **Supported Interfaces:** Protocol bindings (JSON-RPC, gRPC, HTTP+JSON)
- **Capabilities:** streaming, pushNotifications, extensions, stateTransitionHistory
- **Skills:** Specific agent abilities with descriptions, examples
- **Security Schemes:** Authentication requirements (OAuth2, API Key, etc.)
- **Default Input/Output Modes:** Media types supported

### 6.3. Agent Card Signing (Section 8.4)
- Optional digital signatures via JWS (RFC 7515)
- Ensures authenticity and integrity
- Uses JSON Canonicalization Scheme (RFC 8785)

---

## 7. Security (Section 13)

### 7.1. Authorization Scoping (Section 13.1)
- **MUST** scope results to caller's authorization boundaries
- Authorization models are agent-defined:
  - User identity
  - Organizational roles
  - Project/workspace membership
  - Multi-tenant boundaries
- Applies to all operations (List Tasks, Get Task, etc.)

### 7.2. Authentication (Section 7)
- TLS required for production (HTTPS, gRPC with TLS)
- Client authentication via credentials in headers/metadata
- Supports: OAuth2, OpenID Connect, API Keys, Bearer tokens, mTLS
- Out-of-band credential acquisition

### 7.3. Push Notification Security (Section 13.2)
- SSRF prevention (reject private IPs, localhost)
- Client MUST validate webhook authenticity
- Agents MUST include authentication credentials
- Implement timeouts and retry logic

---

## 8. Extensions (Section 4.6)

### 8.1. Purpose
- Additional functionality beyond core spec
- Maintain backward compatibility
- Enable innovation without modifying core
- Pathway to standardization

### 8.2. Extension Declaration
- Agents declare in AgentCard
- URI-based identification
- `required` flag for mandatory extensions
- Clients opt-in via headers/metadata

### 8.3. Extension Points
- **Message Extensions:** Additional context/parameters
- **Artifact Extensions:** Enhanced output metadata
- **Task Extensions:** Custom task information

### 8.4. Versioning
- Version in URI identifier
- Breaking changes require new URI
- Agents ignore unsupported extensions (unless marked required)

---

## 9. Semantic Gap Analysis (Section 1.2, 4.6)

### 9.1. What A2A Provides (Syntactic Level)
- **Message Envelopes:** Structured format for communication
- **Routing:** Task and message delivery
- **Agent Metadata:** Capabilities, skills, endpoints
- **Discovery:** Agent Card mechanism
- **Extensible Primitives:** JSON-based interaction

### 9.2. What A2A Does NOT Provide (Semantic Level)
- **No predefined performatives** (unlike FIPA-ACL: request, inform, query-ref)
- **No shared ontologies** for message meaning
- **No reasoning-level guarantees** about message interpretation
- **No formal semantics** for agent communication acts
- **No verifiable interaction contracts** at the protocol level

### 9.3. Extension Mechanism as Semantic Layer Opportunity
- Extensions can add:
  - Strongly typed message semantics
  - Domain-specific performatives
  - Interaction constraints
  - Verification rules
- Extensions operate at message/artifact/task metadata level
- Can be marked as "required" for enforcement

---

## 10. Applicable Insights for Your Sections

### 10.1. For "Statement of the Problem"
- A2A standardizes only **syntax** (envelopes, routing, discovery)
- Missing: **semantics** (meaning, performatives, constraints)
- Current state: Agents rely on natural language or implicit conventions
- Risk: Ambiguity, non-determinism, hallucinations

### 10.2. For "Justification of Significance"
- A2A is rapidly becoming **dominant open standard** (Linux Foundation)
- Growing ecosystem: Google, IBM merger into A2A
- **Enterprise adoption signals** (authentication, authorization, security)
- **Real-world deployment needs** (async, long-running, human-in-loop)
- Extension mechanism provides **standardized pathway** for semantic layer

### 10.3. For "Scope and Limitations"
- Focus on **Layer 2.5:** Semantic layer between A2A operations and application logic
- Leverage **existing extension mechanism** (Section 4.6)
- Target **horizontal agent-to-agent** communication (not client-to-agent)
- Work within **A2A's protocol binding constraints** (JSON-RPC, gRPC, HTTP)
- Use **metadata and extensions fields** in Message/Task/Artifact

### 10.4. For Your Solution
**Concrete Implementation Path:**
1. Define **typed message extension** with performatives
2. Use **metadata field** for semantic constraints
3. Mark extension as **"required: true"** for enforcement
4. Leverage **Task lifecycle states** for interaction protocols
5. Use **referenceTaskIds** for multi-turn semantic linkage

**Example Semantic Extension Structure:**
```json
{
  "extensions": ["https://research.example.org/extensions/typed-semantics/v1"],
  "metadata": {
    "https://research.example.org/extensions/typed-semantics/v1": {
      "performative": "REQUEST",
      "preconditions": {...},
      "postconditions": {...},
      "constraints": {...}
    }
  }
}
```

---

## 11. Protocol Limitations (Relevant to Your Gap)

### 11.1. Deliberate Design Choices
- **Opaque Execution:** Agents don't share internal reasoning
- **Flexibility over Rigidity:** Natural language allowed
- **Minimal Constraints:** No prescribed message semantics
- **Extension-Based Evolution:** Semantics via extensions, not core

### 11.2. Interoperability Focus
- **Functional Equivalence:** All bindings must behave identically
- **Protocol Neutrality:** Proto files as single source of truth
- **Forward Compatibility:** Ignore unrecognized fields

### 11.3. Your Contribution's Fit
- A2A **explicitly supports** semantic extensions
- Extension mechanism **designed for** this use case
- Protocol **does not conflict** with semantic layer
- Your work **complements** A2A's flexibility goals

---

## 12. Key References for Your Proposal

### 12.1. Relevant Specification Sections
- **Section 1.2:** Guiding Principles (Opaque Execution)
- **Section 3.2.6:** Service Parameters (Extension header transmission)
- **Section 4.6:** Extensions (Declaration, Points, Versioning)
- **Section 4.1.4:** Message (extensions and metadata fields)
- **Section 8:** Agent Discovery (Capabilities declaration)
- **Section 13.1:** Authorization Scoping (Semantic access control)

### 12.2. Key Data Structures
- `Message.extensions`: Array of extension URIs
- `Message.metadata`: Key-value for extension data
- `AgentCard.capabilities.extensions`: Extension declarations
- `AgentExtension.required`: Enforcement flag

### 12.3. Protocol Guarantees You Can Build On
- Extension opt-in mechanism
- Metadata field available in all messages
- Agent Card capability declaration
- Multi-turn context preservation
- Task lifecycle state machine

---

## 13. Research Implications

### 13.1. What Your Semantic Layer Enables
1. **Deterministic Interactions:** Replace NLP ambiguity with typed contracts
2. **Verifiable Communication:** Formal pre/post-conditions
3. **Reliable Delegation:** Clear task semantics
4. **Reduced Hallucinations:** Constrained message spaces
5. **Enhanced Coordination:** Explicit negotiation protocols

### 13.2. How It Fits A2A Philosophy
- **Simple:** Reuses metadata/extensions mechanism
- **Enterprise Ready:** Adds verification without breaking existing systems
- **Async First:** Semantic contracts work with long-running tasks
- **Modality Agnostic:** Type layer over any content type
- **Opaque Execution:** Doesn't require exposing internal reasoning

### 13.3. Implementation Strategy
- Define extension specification document
- Implement as required extension in Agent Card
- Use metadata for semantic payloads
- Leverage Task states for protocol transitions
- Validate at message send/receive boundaries

---

## 14. Competitive Analysis

### 14.1. A2A vs FIPA-ACL/KQML
| Aspect | FIPA-ACL/KQML | A2A (Current) | A2A + Your Semantic Layer |
|--------|---------------|---------------|---------------------------|
| Performatives | Predefined (request, inform, etc.) | None | Extension-defined |
| Ontologies | Built-in | None | Extension-based |
| Flexibility | Low | High | High (opt-in semantics) |
| Adoption | Low (academic) | Growing (industry) | Industry + reliability |
| Interoperability | Limited | Protocol-agnostic | Protocol-agnostic + semantic |

### 14.2. A2A vs MCP
- **MCP:** Client-to-agent tool invocation
- **A2A:** Agent-to-agent collaboration
- **Complementary:** Your semantic layer applies to A2A horizontal communication

---

## 15. Practical Considerations

### 15.1. Compatibility
- Backward compatible: agents without semantic extension still work
- Forward compatible: ignore unrecognized extensions
- Graceful degradation: fall back to natural language if semantic validation fails

### 15.2. Performance
- Metadata overhead minimal (JSON key-value)
- Validation at message boundaries (not per-token)
- Extension loading once per agent session

### 15.3. Scalability
- Extension URIs enable distributed semantic definitions
- Agent Card caching reduces discovery overhead
- Task-based state management handles long-running processes

---

## Conclusion for Your Proposal

**A2A provides the perfect foundation for your semantic layer research because:**

1. **Explicit Extension Mechanism:** Designed for exactly this use case
2. **Industry Momentum:** Linux Foundation, Google, IBM backing
3. **Enterprise Adoption:** Real-world deployment needs
4. **Semantic Gap:** Protocol explicitly leaves semantics to extensions
5. **Standard Compliance:** Your work enhances, not competes with, A2A
6. **Research Impact:** Fills documented gap in emerging standard

**Your contribution moves A2A from syntactic interoperability to semantic reliability—a critical evolution for production multi-agent systems.**
