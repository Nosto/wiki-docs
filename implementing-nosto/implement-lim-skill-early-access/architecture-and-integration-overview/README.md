# Architecture and integration overview

LIM's current architecture combines Nosto Semantic Search with the Core Recommendation Pipeline — Predictive, Semantic, and Visual AI, plus LLMs — to generate responses in real time. The system is horizontally scalable, built to serve high-volume traffic without degradation. LIM also respects all consent and privacy settings configured at the merchant level in Nosto.

#### Integration options

LIM can be integrated in two ways, depending on your system's architecture:

* [**REST API** ](rest-api.md)— suited to direct backend or service-based integrations
* [**MCP (Model Context Protocol)** ](mcp.md)— recommended for agent-native and LLM-driven systems

Both expose the same core capabilities and return equivalent outputs.
