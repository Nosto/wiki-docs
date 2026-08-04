# Implement LIM Skill (Early Access)

### Introduction

Nosto LIM Skill enables third-party agentic systems to deliver real-time, high-quality product discovery and recommendations grounded in live merchant data, behavioral signals, and merchandising rules — without requiring the agent to reason over raw catalogs or ranking logic itself.

LIM acts as a recommendation and intent-resolution layer that third-party agent platforms call on demand via API or MCP. It returns structured, merchant-safe product recommendations suitable for conversational, guided, or autonomous shopping experiences.

#### What problem LIM solves

Modern LLM-based agentic experiences are strong at understanding intent, but struggle to:

* Rank products accurately in real time
* Understand catalog context, inventory status, and SKU performance
* Respect merchant-defined merchandising strategies
* Adapt to live behavioral signals (session-level and user-level)
* Scale reliably across large catalogs and traffic spikes

LIM closes this gap by combining:

* Semantic understanding of user intent
* Real-time behavioral learning
* Merchant-controlled merchandising logic
* Production-grade recommendation infrastructure

This lets partners focus on conversation and orchestration, while LIM handles what to recommend and in what order.

#### Where LIM fits in an agentic architecture

LIM is not a conversational agent, and it does not replace a third-party LLM or orchestration layer.

Instead, LIM is a specialized product discovery and recommendation system, invoked by an agent whenever product discovery, comparison, or suggestion is required.

**Typical flow:**

1. User expresses shopping intent in conversation
2. Partner agent interprets intent and context
3. Agent calls Nosto LIM with structured input
4. LIM returns ranked product recommendations or agentic suggestions
5. Agent presents, explains, or acts on the results

LIM always returns structured output and never takes ownership of the conversation.
