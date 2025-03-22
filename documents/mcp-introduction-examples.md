# MCP Introduction and Examples

## Table of Contents
- [MCP Introduction and Examples](#mcp-introduction-and-examples)
  - [Table of Contents](#table-of-contents)
  - [Introduction to MCP](#introduction-to-mcp)
    - [What is MCP?](#what-is-mcp)
    - [Why Use MCP?](#why-use-mcp)
    - [MCP Architecture](#mcp-architecture)
    - [MCP Capabilities](#mcp-capabilities)
  - [Example 1: Sequential Thinking MCP](#example-1-sequential-thinking-mcp)
    - [What It Does](#what-it-does)
    - [Installation](#installation)
    - [Example Usage](#example-usage)
    - [Benefits](#benefits)
  - [Example 2: Browser Tools MCP](#example-2-browser-tools-mcp)
    - [What It Does](#what-it-does-1)
    - [Installation](#installation-1)
    - [Example Usage](#example-usage-1)
    - [Benefits](#benefits-1)
  - [Example 3: File System MCP](#example-3-file-system-mcp)
    - [What It Does](#what-it-does-2)
    - [Installation](#installation-2)
    - [Example Usage](#example-usage-2)
    - [Benefits](#benefits-2)
  - [Example 4: Image Generation MCP](#example-4-image-generation-mcp)
    - [What It Does](#what-it-does-3)
    - [Implementation Example](#implementation-example)
    - [Benefits](#benefits-3)
  - [Example 5: Database MCP](#example-5-database-mcp)
    - [What It Does](#what-it-does-4)
    - [Implementation Example](#implementation-example-1)
    - [Benefits](#benefits-4)
  - [Example 6: GitHub MCP](#example-6-github-mcp)
    - [What It Does](#what-it-does-5)
    - [Installation](#installation-3)
    - [Example Usage](#example-usage-3)
    - [Benefits](#benefits-5)
  - [Choosing the Right MCP for Your Workflow](#choosing-the-right-mcp-for-your-workflow)
  - [MCP Integration Patterns](#mcp-integration-patterns)
    - [Sequential Processing](#sequential-processing)
    - [Tool Augmentation](#tool-augmentation)
    - [Context Enrichment](#context-enrichment)
  - [Where to Find More MCP Servers](#where-to-find-more-mcp-servers)

## Introduction to MCP

### What is MCP?

The Model Context Protocol (MCP) is an open standard introduced by Anthropic in late 2024 that standardizes how AI models connect to external data sources and tools. Think of MCP like a universal connector or "USB-C port" for AI systems - it provides a standardized way for AI assistants to interact with various data sources, tools, and services without requiring custom integration code for each combination.

MCP addresses a fundamental challenge in AI development: connecting AI models to the data and functionality they need to be truly helpful. Instead of building one-off integrations for every service and data source, developers can implement the MCP standard once and gain access to a growing ecosystem of interoperable tools.

### Why Use MCP?

MCP offers several compelling benefits:

1. **Standardization**: Establish a common protocol across different AI systems and data sources
2. **Reduced Integration Work**: Implement once, connect to many different services
3. **Enhanced AI Capabilities**: Give AI models access to real-time data and specialized tools
4. **Security**: Define clear access boundaries and permission models
5. **Composability**: Mix and match different MCP servers for customized workflows
6. **Community Ecosystem**: Leverage pre-built MCP servers from a growing community

For developers, MCP dramatically simplifies the process of extending AI capabilities. For users, it means AI assistants can do more useful things with less friction.

### MCP Architecture

MCP follows a client-server architecture with several key components:

- **MCP Hosts**: Applications or environments running AI models (e.g., Cursor, Claude Desktop)
- **MCP Clients**: Components within hosts that communicate with MCP servers
- **MCP Servers**: Programs that expose functionality and data through the protocol
- **Resources**: Data sources like files, databases, or APIs
- **Tools**: Functions that can be called by the AI to perform actions
- **Prompts**: Reusable templates for interactions

This architecture creates a clear separation of concerns between the AI model, the interface, and the external systems it needs to access.

### MCP Capabilities

MCP servers can provide three main types of capabilities:

1. **Resources**: Any data that can be read by clients and used as context
   - Document content
   - Database results
   - API responses
   - File contents

2. **Tools**: Functions that perform actions when called
   - Formatting code
   - Generating images
   - Querying databases
   - Interacting with APIs

3. **Prompts**: Reusable templates to help accomplish specific tasks
   - Structured analysis patterns
   - Specialized workflows
   - Domain-specific interactions

Now let's explore practical examples of MCP servers and how they enhance AI workflows.

## Example 1: Sequential Thinking MCP

The Sequential Thinking MCP is a popular tool that helps AI assistants break down complex problems into sequential steps.

### What It Does

This MCP server forces the AI to think step by step through a problem, improving reasoning and reducing errors. It's especially useful for:
- Complex coding tasks
- Mathematical problems
- Logical reasoning
- Planning multi-stage projects

### Installation

```bash
# Install via npx
npx @smithery-ai/server-sequential-thinking
```

In Cursor or Claude, add as an MCP server:
- **Name**: Sequential Thinking
- **Type**: Command
- **Command**: `npx @smithery-ai/server-sequential-thinking`

### Example Usage

When working with this MCP, you might prompt:

```
Using sequential thinking, help me debug why this React component is causing a memory leak.
```

The MCP will guide the AI to:
1. First analyze the component structure
2. Identify effect hooks and state management
3. Check cleanup functions
4. Explore dependency arrays
5. Suggest specific fixes

### Benefits

- Produces more thorough, methodical analysis
- Catches edge cases that might be missed
- Explains reasoning clearly at each step
- Helps with complex debugging scenarios

## Example 2: Browser Tools MCP

The Browser Tools MCP gives AI assistants access to browser functionality, including console logs, network requests, and DOM inspection.

### What It Does

This powerful MCP allows the AI to:
- Read browser console logs
- Access network requests and responses
- Capture screenshots
- Select and modify DOM elements
- Debug web applications

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/browser-tools/mcp-browser-tools
   ```

2. Install Chrome extension:
   - Load the `/chrome-extension` folder as an unpacked extension

3. Add the MCP server:
   - **Name**: Browser Tools
   - **Type**: Command
   - **Command**: `npx @agentes/browser-tools-server`

### Example Usage

With this MCP, you can ask:

```
Check the console logs and tell me if there are any errors on this page.
```

Or:

```
Select the login form on this page and suggest how to improve its accessibility.
```

### Benefits

- Dramatically improves web development workflows
- Enables real-time debugging
- Allows the AI to "see" and interact with the page
- Supports UI/UX improvement suggestions based on actual DOM

## Example 3: File System MCP

The File System MCP provides secure, controlled access to files and directories on your system.

### What It Does

This MCP allows AI assistants to:
- List directory contents
- Read file contents
- Create or modify files (with proper permissions)
- Search for files matching criteria

### Installation

```bash
# Install via npx
npx @modelcontextprotocol/filesystem
```

In Cursor or Claude, add as an MCP server:
- **Name**: Filesystem
- **Type**: Command
- **Command**: `npx @modelcontextprotocol/filesystem`

### Example Usage

With this MCP, you can ask:

```
List all JavaScript files in my current project that use async/await.
```

Or:

```
Read my package.json file and suggest dependency updates.
```

### Benefits

- Contextual awareness of project structure
- Ability to work with actual files rather than snippets
- Safe, permission-based file access
- Faster project navigation and analysis

## Example 4: Image Generation MCP

An Image Generation MCP connects AI assistants to text-to-image models, allowing them to create images during conversations.

### What It Does

This type of MCP enables AI to:
- Generate images from text descriptions
- Create visual assets for projects
- Visualize concepts or ideas
- Produce illustrations for content

### Implementation Example

Here's a simplified example of how to create an Image Generation MCP using Replicate's API:

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

// Create MCP server
const server = new McpServer({ 
  name: "ImageGenerator", 
  version: "1.0.0" 
});

// Image generation tool
server.tool(
  "generate_image",
  {
    prompt: z.string().describe("Text description of image to generate"),
    size: z.enum(["small", "medium", "large"]).optional().describe("Image size")
  },
  async ({ prompt, size = "medium" }) => {
    // Convert size to dimensions
    const dimensions = {
      small: "512x512",
      medium: "768x768",
      large: "1024x1024"
    }[size];
    
    try {
      // In a real implementation, call Replicate or another image API
      const imageUrl = `https://example.com/generated-image?prompt=${encodeURIComponent(prompt)}&size=${dimensions}`;
      
      return {
        content: [{ 
          type: "text", 
          text: `Generated image: ${imageUrl}`
        }]
      };
    } catch (error) {
      return {
        content: [{ 
          type: "text", 
          text: `Error generating image: ${error.message}`
        }]
      };
    }
  }
);

// Start server
const transport = new StdioServerTransport();
server.connect(transport).catch(console.error);
```

### Benefits

- Enhances creative workflows
- Provides visual representation of concepts
- Useful for design ideation
- Speeds up asset creation for projects

## Example 5: Database MCP

Database MCPs allow AI assistants to securely query and interact with database systems.

### What It Does

This type of MCP enables AI to:
- Query databases with proper validation
- Explore database schemas
- Analyze data patterns
- Generate insights from actual data

### Implementation Example

Here's a simple example for SQLite:

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";
import sqlite3 from "sqlite3";
import { open } from "sqlite";

// Create MCP server
const server = new McpServer({ 
  name: "SQLiteTools", 
  version: "1.0.0" 
});

let db;

// Initialize database connection
async function initDb(dbPath) {
  db = await open({
    filename: dbPath,
    driver: sqlite3.Database
  });
}

// Add tool for querying
server.tool(
  "query_database",
  {
    query: z.string().describe("SQL query to execute (SELECT only)")
  },
  async ({ query }) => {
    if (!query.toLowerCase().trim().startsWith("select")) {
      throw new Error("Only SELECT queries are allowed for security reasons");
    }
    
    try {
      if (!db) {
        await initDb("./database.sqlite");
      }
      
      const results = await db.all(query);
      
      return {
        content: [{ 
          type: "text", 
          text: `Query results:\n\n${JSON.stringify(results, null, 2)}`
        }]
      };
    } catch (error) {
      return {
        content: [{ 
          type: "text", 
          text: `Database error: ${error.message}`
        }]
      };
    }
  }
);

// Start server
const transport = new StdioServerTransport();
server.connect(transport).catch(console.error);
```

### Benefits

- Works with real, up-to-date data
- Enables data-driven insights
- Supports database exploration and analysis
- Secure, read-only access by default

## Example 6: GitHub MCP

The GitHub MCP connects AI assistants to GitHub repositories and functionality.

### What It Does

This MCP allows AI to:
- Access repository contents
- Search code and issues
- Create or update files
- View pull requests and commits

### Installation

```bash
# Install via npx
npx @modelcontextprotocol/github
```

In Cursor or Claude, add as an MCP server:
- **Name**: GitHub
- **Type**: Command
- **Command**: `npx @modelcontextprotocol/github`

### Example Usage

With this MCP, you can ask:

```
Find all TODOs in our codebase and create issues for them.
```

Or:

```
Show me the most recent pull requests and summarize the changes.
```

### Benefits

- Integrates development workflows
- Provides repository context for better code suggestions
- Automates common GitHub tasks
- Keeps AI aware of project history and evolution

## Choosing the Right MCP for Your Workflow

When selecting MCP servers for your workflow, consider:

1. **Core Needs**: What external data or functionality is most critical?
2. **Security Requirements**: What level of access control do you need?
3. **Integration Points**: Which systems do you use most frequently?
4. **Workflow Patterns**: How do you typically interact with your AI assistant?

For different scenarios:

**Web Development**
- Browser Tools MCP
- GitHub MCP
- File System MCP

**Data Analysis**
- Database MCP
- CSV/Excel MCP
- Visualization MCP

**Content Creation**
- Image Generation MCP
- Document Management MCP
- Research/Search MCP

## MCP Integration Patterns

MCP servers can be combined in powerful ways:

### Sequential Processing

Chain MCP servers together for multi-step workflows:
1. Research a topic using a Search MCP
2. Generate an outline using Sequential Thinking MCP
3. Create illustrations with an Image Generation MCP
4. Save the result with a File System MCP

### Tool Augmentation

Use one MCP to enhance another:
- Browser Tools MCP to provide visual context for a Code Analysis MCP
- Database MCP to provide data for a Visualization MCP

### Context Enrichment

Multiple MCPs can provide complementary context:
- File System MCP shows local files
- GitHub MCP shows remote repository
- Documentation MCP provides reference material

## Where to Find More MCP Servers

The MCP ecosystem is growing rapidly. Explore these sources for more servers:

1. **Official Repository**: [GitHub - modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)
2. **Smithery.ai**: [https://smithery.ai](https://smithery.ai)
3. **MCP.so**: [https://mcp.so](https://mcp.so)
4. **Community Collections**: [GitHub - awesome-mcp-servers](https://github.com/appcypher/awesome-mcp-servers)
5. **Cursor Directory**: Built into Cursor's MCP settings

Many MCP servers are also available through npm or pip, making installation straightforward:

```bash
# Install via npm/npx
npx @namespace/server-name

# Install via pip
pip install mcp-server-name
```

As more developers build and share MCP servers, the ecosystem will continue to expand, making AI assistants increasingly capable and useful for a wide range of tasks and workflows.