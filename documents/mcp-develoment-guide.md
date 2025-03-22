# MCP Development Guide

## Table of Contents
- [MCP Development Guide](#mcp-development-guide)
  - [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Prerequisites](#prerequisites)
  - [Creating an MCP Server for Cursor](#creating-an-mcp-server-for-cursor)
    - [TypeScript Implementation for Cursor](#typescript-implementation-for-cursor)
      - [Project Setup](#project-setup)
      - [Basic Server Implementation](#basic-server-implementation)
      - [Build and Run](#build-and-run)
      - [Advanced Example: File Analyzer Tool](#advanced-example-file-analyzer-tool)
    - [Python Implementation for Cursor](#python-implementation-for-cursor)
      - [Project Setup](#project-setup-1)
  - [Creating an MCP Server for Claude](#creating-an-mcp-server-for-claude)
    - [TypeScript Implementation for Claude](#typescript-implementation-for-claude)
      - [Project Setup](#project-setup-2)
      - [Basic Server Implementation](#basic-server-implementation-1)
      - [Build and Configure for Claude](#build-and-configure-for-claude)
    - [Python Implementation for Claude](#python-implementation-for-claude)
      - [Project Setup](#project-setup-3)
  - [Using Cursor to Build an MCP Server](#using-cursor-to-build-an-mcp-server)
    - [Setting Up a New Project](#setting-up-a-new-project)
    - [Leveraging Cursor for Implementation Details](#leveraging-cursor-for-implementation-details)
    - [Testing in Cursor](#testing-in-cursor)
  - [Adapting Existing MCP Servers](#adapting-existing-mcp-servers)
    - [Finding MCP Servers to Adapt](#finding-mcp-servers-to-adapt)
    - [Adapting a GitHub MCP Server Example](#adapting-a-github-mcp-server-example)
    - [Security Considerations When Adapting Servers](#security-considerations-when-adapting-servers)
  - [Best Practices](#best-practices)
    - [MCP Server Design](#mcp-server-design)
    - [Tools Implementation](#tools-implementation)
    - [Security Best Practices](#security-best-practices)
  - [Testing and Debugging](#testing-and-debugging)
    - [Testing MCP Servers](#testing-mcp-servers)
    - [Debugging Tips](#debugging-tips)
  - [Example Projects](#example-projects)
    - [Simple Note-taking MCP Server](#simple-note-taking-mcp-server)
    - [Advanced Database Query MCP Server](#advanced-database-query-mcp-server)

## Introduction

Model Context Protocol (MCP) servers enable AI tools like Cursor and Claude to access external data sources and tools. This guide will walk you through building custom MCP servers for both platforms, with practical examples and best practices.

## Prerequisites

Before creating an MCP server, ensure you have:

1. **Development Environment**:
   - Node.js 20+ (for TypeScript/JavaScript)
   - Python 3.10+ (for Python)
   - Code editor (preferably Cursor)
   - Git for version control

2. **MCP Knowledge**:
   - Basic understanding of the MCP protocol
   - Familiarity with either TypeScript or Python
   - Understanding of your target platform (Cursor or Claude)

3. **Required Packages**:
   - For TypeScript: `@modelcontextprotocol/sdk` and `zod`
   - For Python: `mcp` package and `httpx`

## Creating an MCP Server for Cursor

Cursor supports MCP servers which provide tools and resources through a standardized interface. Here are implementations in different languages:

### TypeScript Implementation for Cursor

#### Project Setup

1. Create a new directory and initialize:
   ```bash
   mkdir cursor-mcp-server
   cd cursor-mcp-server
   npm init -y
   ```

2. Install dependencies:
   ```bash
   npm install @modelcontextprotocol/sdk zod
   npm install --save-dev typescript @types/node
   ```

3. Create a TypeScript configuration file (`tsconfig.json`):
   ```json
   {
     "compilerOptions": {
       "target": "ES2022",
       "module": "Node16",
       "moduleResolution": "Node16",
       "outDir": "./dist",
       "rootDir": "./src",
       "strict": true,
       "esModuleInterop": true,
       "skipLibCheck": true,
       "forceConsistentCasingInFileNames": true
     },
     "include": ["src/**/*"]
   }
   ```

4. Update `package.json`:
   ```json
   {
     "type": "module",
     "scripts": {
       "build": "tsc",
       "start": "node dist/index.js"
     }
   }
   ```

#### Basic Server Implementation

Create a file at `src/index.ts`:

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

// Create an MCP server
const server = new McpServer({ 
  name: "CursorHelperTools", 
  version: "1.0.0" 
});

// Add a simple greeting tool
server.tool(
  "greet",
  { 
    name: z.string().describe("The name to greet"),
    formal: z.boolean().optional().describe("Whether to use formal greeting")
  },
  async ({ name, formal = false }) => {
    const greeting = formal ? `Hello, ${name}. It's a pleasure to meet you.` : `Hi ${name}!`;
    
    return {
      content: [{ 
        type: "text", 
        text: greeting
      }]
    };
  }
);

// Start the server using stdio transport
const transport = new StdioServerTransport();
server.connect(transport).catch(error => {
  console.error("Server error:", error);
  process.exit(1);
});
```

#### Build and Run

1. Compile the TypeScript code:
   ```bash
   npm run build
   ```

2. In Cursor, go to Settings → Features → MCP Server
3. Add a new MCP server:
   - Name: Helper Tools
   - Type: Command
   - Command: `node /path/to/cursor-mcp-server/dist/index.js`
4. Click "Add"

#### Advanced Example: File Analyzer Tool

Let's add a more useful tool that analyzes code files:

```typescript
// Add to src/index.ts

import { promises as fs } from 'fs';
import path from 'path';

// File analyzer tool
server.tool(
  "analyze_file",
  { 
    filepath: z.string().describe("Path to the file to analyze"),
  },
  async ({ filepath }) => {
    try {
      // Read file content
      const content = await fs.readFile(filepath, 'utf8');
      
      // Basic stats
      const lines = content.split('\n');
      const lineCount = lines.length;
      const charCount = content.length;
      const wordCount = content.split(/\s+/).filter(Boolean).length;
      
      // Determine file type based on extension
      const ext = path.extname(filepath).toLowerCase();
      let fileType = 'Unknown';
      
      // Simple file type detection
      if (['.js', '.ts', '.jsx', '.tsx'].includes(ext)) {
        fileType = 'JavaScript/TypeScript';
      } else if (['.py'].includes(ext)) {
        fileType = 'Python';
      } else if (['.html', '.htm', '.xml'].includes(ext)) {
        fileType = 'HTML/XML';
      } else if (['.css', '.scss', '.sass', '.less'].includes(ext)) {
        fileType = 'CSS/Stylesheet';
      } else if (['.json'].includes(ext)) {
        fileType = 'JSON';
      } else if (['.md', '.markdown'].includes(ext)) {
        fileType = 'Markdown';
      }
      
      // Create analysis text
      const analysis = `
File Analysis for: ${path.basename(filepath)}
---------------------------------
Type: ${fileType}
Lines: ${lineCount}
Words: ${wordCount}
Characters: ${charCount}
---------------------------------
Sample Content (first 5 lines):
${lines.slice(0, 5).join('\n')}
...
`;
      
      return {
        content: [{ 
          type: "text", 
          text: analysis
        }]
      };
    } catch (error: any) {
      return {
        content: [{ 
          type: "text", 
          text: `Error analyzing file: ${error.message}`
        }]
      };
    }
  }
);
```

### Python Implementation for Cursor

#### Project Setup

1. Create a new directory and set up a virtual environment:
   ```bash
   mkdir cursor-mcp-python
   cd cursor-mcp-python
   python -m venv venv
   # Windows
   venv\Scripts\activate
   # macOS/Linux
   source venv/bin/activate
   ```

2. Install dependencies:
   ```bash
   pip install mcp httpx
   ```

3. Create a basic server file (`server.py`):

```python
from mcp.fastmcp import FastMCP, tool
from typing import Literal, Optional
import os

# Create the MCP server
app = FastMCP(
    name="PythonTools",
    version="1.0.0"
)

@tool
async def greet(
    name: str,
    formal: Optional[bool] = False
) -> str:
    """
    Generate a greeting for a person.
    
    Args:
        name: The name to greet
        formal: Whether to use formal greeting
        
    Returns:
        The greeting message
    """
    if formal:
        return f"Hello, {name}. It's a pleasure to meet you."
    else:
        return f"Hi {name}!"

app.add_tool(greet)

@tool
async def analyze_file(
    filepath: str
) -> str:
    """
    Analyze a file and provide statistics.
    
    Args:
        filepath: Path to the file to analyze
        
    Returns:
        Analysis information as text
    """
    try:
        # Read file content
        with open(filepath, 'r', encoding='utf-8') as file:
            content = file.read()
        
        # Basic stats
        lines = content.splitlines()
        line_count = len(lines)
        char_count = len(content)
        word_count = len(content.split())
        
        # Determine file type based on extension
        import os
        ext = os.path.splitext(filepath)[1].lower()
        
        file_type = 'Unknown'
        if ext in ('.js', '.ts', '.jsx', '.tsx'):
            file_type = 'JavaScript/TypeScript'
        elif ext in ('.py'):
            file_type = 'Python'
        elif ext in ('.html', '.htm', '.xml'):
            file_type = 'HTML/XML'
        elif ext in ('.css', '.scss', '.sass', '.less'):
            file_type = 'CSS/Stylesheet'
        elif ext in ('.json'):
            file_type = 'JSON'
        elif ext in ('.md', '.markdown'):
            file_type = 'Markdown'
        
        # Create analysis text
        analysis = f"""
File Analysis for: {os.path.basename(filepath)}
---------------------------------
Type: {file_type}
Lines: {line_count}
Words: {word_count}
Characters: {char_count}
---------------------------------
Sample Content (first 5 lines):
{os.linesep.join(lines[:5])}
...
"""
        return analysis
    except Exception as e:
        return f"Error analyzing file: {str(e)}"

app.add_tool(analyze_file)

if __name__ == "__main__":
    app.run()

# To use in Cursor:
# 1. Activate your virtual environment
# 2. In Cursor, go to Settings → Features → MCP Server
# 3. Add new MCP server:
#    - Name: Python Tools
#    - Type: Command
#    - Command: python /path/to/cursor-mcp-python/server.py
# 4. Click "Add"

### Cloudflare Worker Implementation for Cursor

Cloudflare Workers provide a simple way to host MCP servers. This approach is especially useful for deploying MCP servers that can be accessed remotely.

#### Project Setup

1. Create a new Cloudflare Worker project:
   ```bash
   npm create cloudflare@latest
   ```

2. During setup:
   - Name your project (e.g., "cursor-mcp-cloudflare")
   - Choose "Hello World" example
   - Select TypeScript
   - Choose whether to use Git
   - Choose deployment options (you can deploy later)

3. Navigate to your project folder and install the MCP SDK:
   ```bash
   cd cursor-mcp-cloudflare
   npm install @modelcontextprotocol/sdk zod
   ```

4. Replace the content of `src/index.ts` with:

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

// Create an MCP server
const server = new McpServer({ 
  name: "CloudflareMcpTools", 
  version: "1.0.0" 
});

// Add a weather tool
server.tool(
  "get_weather",
  { 
    location: z.string().describe("City name or location"),
    units: z.enum(["celsius", "fahrenheit"]).optional()
      .describe("Temperature units (default: celsius)")
  },
  async ({ location, units = "celsius" }) => {
    try {
      // In a real implementation, you would call a weather API here
      // This is a mock implementation for demonstration
      const mockWeather = {
        location,
        temperature: units === "celsius" ? 22 : 72,
        condition: "Partly Cloudy",
        humidity: 65,
        windSpeed: 10,
        units
      };
      
      const response = `
Weather for ${mockWeather.location}:
Temperature: ${mockWeather.temperature}° ${units === "celsius" ? "C" : "F"}
Condition: ${mockWeather.condition}
Humidity: ${mockWeather.humidity}%
Wind Speed: ${mockWeather.windSpeed} km/h
`;
      
      return {
        content: [{ 
          type: "text", 
          text: response
        }]
      };
    } catch (error: any) {
      return {
        content: [{ 
          type: "text", 
          text: `Error getting weather: ${error.message}`
        }]
      };
    }
  }
);

// Start the server
const transport = new StdioServerTransport();
server.connect(transport).catch(error => {
  console.error("Server error:", error);
  process.exit(1);
});
```

5. Build the worker:
   ```bash
   npm run build
   ```

6. To use in Cursor:
   - Go to Settings → Features → MCP Server
   - Add a new MCP server:
     - Name: Cloudflare MCP
     - Type: Command
     - Command: `node /path/to/cursor-mcp-cloudflare/dist/index.js`
   - Click "Add"

## Creating an MCP Server for Claude

Claude Desktop uses a configuration file to set up MCP servers. Here's how to create custom MCP servers for Claude:

### TypeScript Implementation for Claude

#### Project Setup

1. Create a new directory and initialize:
   ```bash
   mkdir claude-mcp-server
   cd claude-mcp-server
   npm init -y
   ```

2. Install dependencies:
   ```bash
   npm install @modelcontextprotocol/sdk zod
   npm install --save-dev typescript @types/node
   ```

3. Create a TypeScript configuration file (`tsconfig.json`):
   ```json
   {
     "compilerOptions": {
       "target": "ES2022",
       "module": "Node16",
       "moduleResolution": "Node16",
       "outDir": "./dist",
       "rootDir": "./src",
       "strict": true,
       "esModuleInterop": true,
       "skipLibCheck": true,
       "forceConsistentCasingInFileNames": true
     },
     "include": ["src/**/*"]
   }
   ```

4. Update `package.json`:
   ```json
   {
     "type": "module",
     "scripts": {
       "build": "tsc",
       "start": "node dist/index.js"
     }
   }
   ```

#### Basic Server Implementation

Create a file at `src/index.ts`:

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

// Create an MCP server
const server = new McpServer({ 
  name: "ClaudeHelperTools", 
  version: "1.0.0" 
});

// Document search tool
server.tool(
  "search_documents",
  { 
    query: z.string().describe("Search query"),
    max_results: z.number().min(1).max(10).optional()
      .describe("Maximum number of results to return (default: 5)")
  },
  async ({ query, max_results = 5 }) => {
    try {
      // In a real implementation, you would search actual documents
      // This is a mock implementation for demonstration
      const mockResults = [
        { title: "Getting Started Guide", path: "/docs/getting-started.md", relevance: 0.92 },
        { title: "API Reference", path: "/docs/api-reference.md", relevance: 0.85 },
        { title: "Configuration Options", path: "/docs/configuration.md", relevance: 0.78 },
        { title: "Troubleshooting", path: "/docs/troubleshooting.md", relevance: 0.72 },
        { title: "Advanced Features", path: "/docs/advanced-features.md", relevance: 0.65 },
        { title: "User Guide", path: "/docs/user-guide.md", relevance: 0.60 },
        { title: "FAQ", path: "/docs/faq.md", relevance: 0.55 }
      ];
      
      // Filter results based on query (in a real implementation, this would be more sophisticated)
      const filteredResults = mockResults
        .filter(doc => doc.title.toLowerCase().includes(query.toLowerCase()) || 
                 doc.path.toLowerCase().includes(query.toLowerCase()))
        .slice(0, max_results);
      
      if (filteredResults.length === 0) {
        return {
          content: [{ 
            type: "text", 
            text: `No documents found matching query: "${query}"`
          }]
        };
      }
      
      const resultText = filteredResults
        .map((doc, index) => `${index + 1}. ${doc.title} (${doc.path}) - Relevance: ${doc.relevance.toFixed(2)}`)
        .join('\n');
      
      return {
        content: [{ 
          type: "text", 
          text: `Search results for "${query}":\n\n${resultText}`
        }]
      };
    } catch (error: any) {
      return {
        content: [{ 
          type: "text", 
          text: `Error searching documents: ${error.message}`
        }]
      };
    }
  }
);

// Start the server
const transport = new StdioServerTransport();
server.connect(transport).catch(error => {
  console.error("Server error:", error);
  process.exit(1);
});
```

#### Build and Configure for Claude

1. Compile the TypeScript code:
   ```bash
   npm run build
   ```

2. Configure Claude Desktop to use your MCP server:

   a. Locate the Claude Desktop configuration file:
      - Windows: `%APPDATA%\Claude\claude_desktop_config.json`
      - macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
      - Linux: `~/.config/Claude/claude_desktop_config.json`

   b. Edit the configuration file (create it if it doesn't exist):
      ```json
      {
        "mcpServers": {
          "claude-helper-tools": {
            "command": "node",
            "args": [
              "/path/to/claude-mcp-server/dist/index.js"
            ]
          }
        }
      }
      ```

3. Restart Claude Desktop

### Python Implementation for Claude

#### Project Setup

1. Create a new directory and set up a virtual environment:
   ```bash
   mkdir claude-mcp-python
   cd claude-mcp-python
   python -m venv venv
   # Windows
   venv\Scripts\activate
   # macOS/Linux
   source venv/bin/activate
   ```

2. Install dependencies:
   ```bash
   pip install mcp httpx
   ```

3. Create a basic server file (`server.py`):

```python
from mcp.fastmcp import FastMCP, tool
from typing import Optional, List, Dict, Any
import json

# Create the MCP server
app = FastMCP(
    name="ClaudePythonTools",
    version="1.0.0"
)

@tool
async def search_documents(
    query: str,
    max_results: Optional[int] = 5
) -> str:
    """
    Search for documents matching a query.
    
    Args:
        query: Search query
        max_results: Maximum number of results to return
        
    Returns:
        Search results as text
    """
    try:
        # In a real implementation, you would search actual documents
        # This is a mock implementation for demonstration
        mock_results = [
            {"title": "Getting Started Guide", "path": "/docs/getting-started.md", "relevance": 0.92},
            {"title": "API Reference", "path": "/docs/api-reference.md", "relevance": 0.85},
            {"title": "Configuration Options", "path": "/docs/configuration.md", "relevance": 0.78},
            {"title": "Troubleshooting", "path": "/docs/troubleshooting.md", "relevance": 0.72},
            {"title": "Advanced Features", "path": "/docs/advanced-features.md", "relevance": 0.65},
            {"title": "User Guide", "path": "/docs/user-guide.md", "relevance": 0.60},
            {"title": "FAQ", "path": "/docs/faq.md", "relevance": 0.55}
        ]
        
        # Filter results based on query (in a real implementation, this would be more sophisticated)
        filtered_results = [
            doc for doc in mock_results
            if query.lower() in doc["title"].lower() or query.lower() in doc["path"].lower()
        ][:max_results]
        
        if not filtered_results:
            return f'No documents found matching query: "{query}"'
        
        result_text = '\n'.join([
            f"{i+1}. {doc['title']} ({doc['path']}) - Relevance: {doc['relevance']:.2f}"
            for i, doc in enumerate(filtered_results)
        ])
        
        return f'Search results for "{query}":\n\n{result_text}'
    except Exception as e:
        return f"Error searching documents: {str(e)}"

app.add_tool(search_documents)

@tool
async def summarize_document(
    document_path: str,
    max_length: Optional[int] = 500
) -> str:
    """
    Generate a summary of a document.
    
    Args:
        document_path: Path to the document
        max_length: Maximum summary length in characters
        
    Returns:
        Document summary
    """
    try:
        # Mock implementation - in real world, you would read and summarize the actual document
        document_summaries = {
            "/docs/getting-started.md": "This guide introduces the basics of the platform, covering installation, initial setup, and first steps. It's suitable for new users who want to quickly get up and running.",
            "/docs/api-reference.md": "Comprehensive API documentation including endpoints, parameters, response formats, error codes, and examples. Essential reference for developers integrating with the platform.",
            "/docs/configuration.md": "Detailed explanation of all configuration options, including environment variables, config files, and runtime settings. Includes best practices and recommendations."
        }
        
        if document_path in document_summaries:
            summary = document_summaries[document_path]
            if len(summary) > max_length:
                summary = summary[:max_length] + "..."
            return f"Summary of {document_path}:\n\n{summary}"
        else:
            return f"Document not found: {document_path}"
    except Exception as e:
        return f"Error summarizing document: {str(e)}"

app.add_tool(summarize_document)

if __name__ == "__main__":
    app.run()
```

4. Configure Claude Desktop:

   a. Locate the Claude Desktop configuration file as described above
   
   b. Edit the configuration file:
      ```json
      {
        "mcpServers": {
          "claude-python-tools": {
            "command": "python",
            "args": [
              "/path/to/claude-mcp-python/server.py"
            ]
          }
        }
      }
      ```

5. Restart Claude Desktop

## Using Cursor to Build an MCP Server

Cursor's AI capabilities make it an excellent tool for developing MCP servers. Here's how to use Cursor to build an MCP server efficiently:

### Setting Up a New Project

1. Create a new folder in Cursor for your MCP server
2. Use Cursor's terminal to initialize your project:
   ```bash
   # For TypeScript
   npm init -y
   npm install @modelcontextprotocol/sdk zod
   npm install --save-dev typescript @types/node
   
   # For Python
   python -m venv venv
   source venv/bin/activate  # or venv\Scripts\activate on Windows
   pip install mcp httpx
   ```

3. Ask Cursor to generate a basic MCP server structure:

```
Prompt: "Help me create a basic TypeScript MCP server with the following tools:
1. A 'format_code' tool that takes code as input and formats it nicely
2. A 'translate' tool that takes text and a target language and returns a translation
Please include proper error handling and type definitions."
```

Cursor will generate the initial code structure that you can refine.

### Leveraging Cursor for Implementation Details

You can ask Cursor to help with specific implementations:

```
Prompt: "Help me implement the 'format_code' tool that I defined earlier. It should:
1. Support JavaScript, TypeScript, Python, and JSON
2. Format code with proper indentation and spacing
3. Handle errors gracefully
4. Return both formatted code and a summary of changes made"
```

### Testing in Cursor

1. After implementing your MCP server, build it:
   ```bash
   # For TypeScript
   npx tsc
   
   # For Python - no build step needed
   ```

2. Add your MCP server to Cursor:
   - Go to Settings → Features → MCP Server
   - Add a new MCP server pointing to your compiled server

3. Test your MCP server in a conversation:
   ```
   Prompt: "I need to format this code:
   function hello(){console.log('hello world');return true;}
   Please use the format_code tool to make it more readable."
   ```

## Adapting Existing MCP Servers

You can adapt existing MCP servers to your needs, which is often faster than building from scratch:

### Finding MCP Servers to Adapt

Good sources include:
- [Official MCP Server repository](https://github.com/modelcontextprotocol/servers)
- [Smithery.ai reference servers](https://github.com/smithery-ai/reference-servers)
- [MCP.so server directory](https://mcp.so)

### Adapting a GitHub MCP Server Example

Let's adapt the GitHub MCP server to create a GitLab MCP server:

1. Clone the original repository:
   ```bash
   git clone https://github.com/modelcontextprotocol/servers
   cd servers/github
   ```

2. Create a new directory for your adaptation:
   ```bash
   mkdir ../../my-gitlab-mcp
   cp -r * ../../my-gitlab-mcp/
   cd ../../my-gitlab-mcp
   ```

3. Modify package.json:
   ```json
   {
     "name": "my-gitlab-mcp",
     "version": "1.0.0",
     "description": "GitLab MCP Server",
     ...
   }
   ```

4. Update the server implementation to use GitLab's API instead of GitHub's:
   - Modify API endpoints
   - Update authentication mechanisms
   - Change field names and data structures as needed

5. Ensure you properly secure the code to remove any potential security issues before using it.

### Security Considerations When Adapting Servers

When adapting existing MCP servers:

1. Review the code for security issues:
   - Look for hardcoded credentials
   - Check for insecure API calls
   - Verify permission handling

2. Scan dependencies for vulnerabilities:
   ```bash
   npm audit
   # or
   pip-audit
   ```

3. Add proper authentication and authorization:
   - Use environment variables for tokens
   - Implement access controls
   - Validate user input

4. Test thoroughly before deploying

## Best Practices

### MCP Server Design

1. **Follow the Single Responsibility Principle**: Each server should focus on a specific domain.
2. **Use Descriptive Tool Names**: Names should clearly indicate functionality.
3. **Provide Detailed Documentation**: Each tool should have comprehensive documentation.
4. **Handle Errors Gracefully**: Provide helpful error messages and avoid crashing.
5. **Implement Proper Validation**: Validate all inputs using schemas (zod for TypeScript, type hints for Python).

### Tools Implementation

1. **Keep Tools Focused**: Each tool should do one thing well.
2. **Provide Clear Parameter Descriptions**: Users and AI should understand what each parameter does.
3. **Use Default Values Wisely**: Provide sensible defaults for optional parameters.
4. **Return Structured Data**: Make output easy to parse and understand.
5. **Implement Timeouts**: Long-running operations should have reasonable timeouts.

### Security Best Practices

1. **Validate All Inputs**: Never trust user input.
2. **Use Environment Variables for Secrets**: Never hardcode credentials.
3. **Implement Access Controls**: Limit what tools can access.
4. **Rate Limit Operations**: Prevent abuse through excessive use.
5. **Audit Tool Calls**: Log actions for security review.

## Testing and Debugging

### Testing MCP Servers

1. **Manual Testing**: Test your server directly in Cursor or Claude.
2. **MCP Inspector**: Use the MCP Inspector tool to test servers before integration:
   ```bash
   npx @modelcontextprotocol/inspector
   ```

3. **Unit Tests**: Write tests for your tools:
   ```typescript
   // Example Jest test for a tool
   test('format_code should correctly format JavaScript', async () => {
     const result = await formatCodeTool({
       code: "function test(){return true;}",
       language: "javascript"
     });
     expect(result).toContain("function test() {");
     expect(result).toContain("  return true;");
   });
   ```

### Debugging Tips

1. **Enable Verbose Logging**:
   ```typescript
   // In TypeScript
   console.debug("Processing request:", request);
   
   # In Python
   import logging
   logging.debug(f"Processing request: {request}")
   ```

2. **Run Standalone**: Test your server outside of Cursor/Claude first.
3. **Check Request/Response Flow**: Log the entire request/response cycle.
4. **Validate Connections**: Ensure transport methods are working correctly.

## Example Projects

### Simple Note-taking MCP Server

```typescript
// src/index.ts
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";
import { promises as fs } from 'fs';
import path from 'path';

// Create an MCP server
const server = new McpServer({ 
  name: "NoteTaker", 
  version: "1.0.0" 
});

// Store notes in memory
const notes: Record<string, string> = {};
let notesDir = './notes';

// Initialize notes directory
const initNotesDir = async () => {
  try {
    await fs.mkdir(notesDir, { recursive: true });
    
    // Load existing notes
    const files = await fs.readdir(notesDir);
    for (const file of files) {
      if (file.endsWith('.txt')) {
        const title = path.basename(file, '.txt');
        const content = await fs.readFile(path.join(notesDir, file), 'utf8');
        notes[title] = content;
      }
    }
  } catch (error) {
    console.error("Error initializing notes directory:", error);
  }
};

// Initialize on startup
initNotesDir();

// Add note tool
server.tool(
  "add_note",
  { 
    title: z.string().describe("Note title"),
    content: z.string().describe("Note content")
  },
  async ({ title, content }) => {
    try {
      notes[title] = content;
      
      // Save to file
      await fs.writeFile(path.join(notesDir, `${title}.txt`), content, 'utf8');
      
      return {
        content: [{ 
          type: "text", 
          text: `Note "${title}" saved successfully.`
        }]
      };
    } catch (error: any) {
      return {
        content: [{ 
          type: "text", 
          text: `Error saving note: ${error.message}`
        }]
      };
    }
  }
);

// Get note tool
server.tool(
  "get_note",
  { 
    title: z.string().describe("Note title")
  },
  async ({ title }) => {
    try {
      if (notes[title]) {
        return {
          content: [{ 
            type: "text", 
            text: `Note: ${title}\n\n${notes[title]}`
          }]
        };
      } else {
        return {
          content: [{ 
            type: "text", 
            text: `Note "${title}" not found.`
          }]
        };
      }
    } catch (error: any) {
      return {
        content: [{ 
          type: "text", 
          text: `Error retrieving note: ${error.message}`
        }]
      };
    }
  }
);

// List notes tool
server.tool(
  "list_notes",
  {},
  async () => {
    try {
      const notesList = Object.keys(notes);
      
      if (notesList.length === 0) {
        return {
          content: [{ 
            type: "text", 
            text: "No notes found."
          }]
        };
      }
      
      const notesText = notesList
        .map((title, index) => `${index + 1}. ${title}`)
        .join('\n');
      
      return {
        content: [{ 
          type: "text", 
          text: `Available notes:\n\n${notesText}`
        }]
      };
    } catch (error: any) {
      return {
        content: [{ 
          type: "text", 
          text: `Error listing notes: ${error.message}`
        }]
      };
    }
  }
);

// Start the server
const transport = new StdioServerTransport();
server.connect(transport).catch(error => {
  console.error("Server error:", error);
  process.exit(1);
});
```

### Advanced Database Query MCP Server

For a more complex example, check out the [MCP SQLite server in the modelcontextprotocol/servers repository](https://github.com/modelcontextprotocol/servers/tree/main/sqlite).

This server allows you to:
- Connect to SQLite databases
- Inspect database schemas
- Run queries with proper validation
- Format results in various formats