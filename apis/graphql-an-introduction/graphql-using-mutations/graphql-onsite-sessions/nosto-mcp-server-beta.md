---
description: >-
  This MCP server provides a comprehensive set of GraphQL tools for integrating
  Nosto's personalization and recommendation engine into commerce applications.
---

# Nosto MCP Server (beta)

{% hint style="danger" %}
Nosto MCP Server is in Beta state, and any generated code should be reviewed.
{% endhint %}

### What is an MCP Server?

**MCP (Model Context Protocol)** is a standardized protocol that allows AI assistants like Claude to connect to external tools, databases and services. Think of it as a bridge that extends AI assistants capabilities with specialized functionality.&#x20;

### Overview of tools

The Nosto MCP server exposes 7 main GraphQL tools that handle different aspects of Nosto Integration:

1. Session Managing - Creating and managing user sessions
2. Event Tracking - Tracking user interactions and behaviour
3. Recommendations - Fetching personalized product recommendations
4. Complete Workflows - End-to-end integration patterns
5. Service Layer - Clean architecture with service classes
6. Cookie Management - Session persistence via cookies
7. Concept Explanation - Educational content about Nosto GraphQL concepts

There are also 2 Documentation tools

1. Feature Documentation - RAG-powered feature documentation search
2. Technical Documentation - RAG-powered code/technical documentation search

### Nosto MCP Server

**Server URL** [https://dev.mcp.nosto.com/mcp](https://dev.mcp.nosto.com/mcp)

Nosto MCP server specializes in:

* Nosto GraphQL  API Integration
* Multi-framework support (React, Next Js, Shopify Hydrogen, etc.)
* Production-ready code generation
* Best practives enforcement
* Complete workflow documantation&#x20;
* Technical documentation from [https://docs.nosto.com](https://docs.nosto.com/)



### How to use Nosto MCP Server

{% hint style="success" %}
Nosto MCP server can be used with any AI Assistant that support Model Context Protocol. Claude code is used in the documentation below.
{% endhint %}

#### Quick Start with Claude Code

{% hint style="warning" %}
Prerequisites: You need access to Claude Code (Anthropic's IDE integration) to use Nosto MCP servers.
{% endhint %}

**Step 1**

**Configure MCP**

Add the Nosto MCP server to your Claude Code configuration:

```
{
  "mcpServers": {
    "nosto-graphql": {
       "type": "http",
       "url": "https://dev.mcp.nosto.com/mcp",
       "env": {}
    }
  }
}

```

**Step 2**

**Verify Connection**

Test the connection by asking Claude:

```
Can you list the available Nosto GraphQL tools?
```

Claude should respond with the 7 GraphQL specific tools and 2 (RAG) documentation tools.

<figure><img src="../../../../.gitbook/assets/Screenshot 2025-09-25 at 16.32.22.png" alt=""><figcaption></figcaption></figure>

**Step 3**

**Start Building**

Now you can request complete integrations:

```
Add Nosto product recommendations to my home page
```

Claude will automatically use the MCP tools to generate complete, production-ready code, including the creation of Nosto specific service, creating a newSession request, storing the sessionId, and calling updateSession and retrieve recommendations.

### Reasons to use MCP Server

MCP server is aware of the step by step workflow of how Nosto should be implemented and how graphQL mutations should look. \
\
This means that asking AI assistant such as:\


<figure><img src="../../../../.gitbook/assets/Screenshot 2025-09-25 at 12.50.51.png" alt=""><figcaption></figcaption></figure>

Will look through multiple tools exposed by the Nosto MCP Server

<figure><img src="../../../../.gitbook/assets/Screenshot 2025-09-25 at 12.52.50.png" alt=""><figcaption></figcaption></figure>

And will generate the Nosto service code which is aware of creating a new session, storing sessionId in the cookie, and using the sessionId for update session mutations and getting recommendations.&#x20;

<figure><img src="../../../../.gitbook/assets/Screenshot 2025-09-25 at 12.53.52.png" alt=""><figcaption></figcaption></figure>

End result is visible Nosto recommendations on home and product pages within minutes.

<figure><img src="../../../../.gitbook/assets/Screenshot 2025-09-25 at 12.56.10.png" alt=""><figcaption></figcaption></figure>

### Available GraphQL Integration Patterns

The MCP server provides **7 specialized graphql tools** that can be combined. to create complete e-commerce solutions.

**Common usage patters:**

1. **Homepage integration**\
   "Add Nosto Recommendation to my React homepage" -> Uses generate\_nosto\_service + generate\_complete\_workflow tools
2. **Product page setup**\
   "Set up Nosto on my Shopify Hydrogen product page" -> Uses generate\_nosto\_service + generate\_complete\_workflow tools + Includes Shopify GID handling
3. **Explanation of the Nosto**\
   "Explain how Nosto can be integrated on my store" -> Uses generate\_complete\_workflow + concept\_explainer



### Feedback

If you have found MCP server useful, or if you found some issues or would like MCP server to support more use-cases please don't hesitate to contact us at platforms@nosto.com&#x20;

\


