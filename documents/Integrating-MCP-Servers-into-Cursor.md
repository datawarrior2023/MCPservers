# Comprehensive Guide to MCP Servers in Cursor

## Table of Contents

- [Comprehensive Guide to MCP Servers in Cursor](#comprehensive-guide-to-mcp-servers-in-cursor)
  - [Table of Contents](#table-of-contents)
  - [Introduction to MCP](#introduction-to-mcp)
    - [Why MCP Matters](#why-mcp-matters)
    - [Key Concepts in MCP](#key-concepts-in-mcp)
    - [Common Use Cases](#common-use-cases)
  - [Prerequisites and System Requirements](#prerequisites-and-system-requirements)
    - [General Requirements](#general-requirements)
    - [For TypeScript/JavaScript MCP Servers](#for-typescriptjavascript-mcp-servers)
    - [For Python MCP Servers](#for-python-mcp-servers)
    - [Optional Tools](#optional-tools)
  - [Finding and Installing Existing MCPs](#finding-and-installing-existing-mcps)
    - [MCP Marketplaces](#mcp-marketplaces)
    - [Basic Steps to Add an MCP to Cursor](#basic-steps-to-add-an-mcp-to-cursor)
    - [Using Smithery CLI (Optional)](#using-smithery-cli-optional)
  - [Popular MCP Examples](#popular-mcp-examples)
    - [Sequential Thinking MCP](#sequential-thinking-mcp)
    - [Browser Tools MCP](#browser-tools-mcp)
    - [Image Generation MCP](#image-generation-mcp)
  - [Creating Your Own MCP Servers](#creating-your-own-mcp-servers)
    - [Basic Cloudflare Worker Setup](#basic-cloudflare-worker-setup)
    - [TypeScript MCP Server](#typescript-mcp-server)
    - [Python MCP Server](#python-mcp-server)
  - [Best Practices for MCP Development](#best-practices-for-mcp-development)
    - [Server Design](#server-design)
    - [Code Structure](#code-structure)
    - [MCP Tool Design](#mcp-tool-design)
  - [MCP Server Documentation Standards](#mcp-server-documentation-standards)
    - [README.md Template](#readmemd-template)
  - [Usage with Cursor](#usage-with-cursor)
  - [Available Tools](#available-tools)
    - [Tool 1: \[Tool Name\]](#tool-1-tool-name)
  - [Configuration](#configuration)
  - [Development](#development)
  - [License](#license)
  - [Contact](#contact)
    - [Common Issues and Solutions](#common-issues-and-solutions)
  - [MCP Resources and Community](#mcp-resources-and-community)
    - [Official Resources](#official-resources)
    - [Community Resources](#community-resources)
    - [Tutorials and Guides](#tutorials-and-guides)
  - [Advanced Use Cases](#advanced-use-cases)
    - [Database Integration](#database-integration)
    - [AI Model Integration](#ai-model-integration)
  - [Appendix: Installation Guides](#appendix-installation-guides)
    - [Installing Node.js](#installing-nodejs)
      - [Windows](#windows)
      - [macOS](#macos)
      - [Linux (Ubuntu/Debian)](#linux-ubuntudebian)
    - [Installing Python and uv](#installing-python-and-uv)
      - [Windows](#windows-1)
      - [macOS](#macos-1)
      - [Linux (Ubuntu/Debian)](#linux-ubuntudebian-1)
    - [Installing Git](#installing-git)
      - [Windows](#windows-2)
      - [macOS](#macos-2)
      - [Linux (Ubuntu/Debian)](#linux-ubuntudebian-2)

## Introduction to MCP

The Model Context Protocol (MCP) is a universal connector for AI coding assistants, introduced by Anthropic in late 2024. Think of it as a USB-C port for AI tools - it provides a standardized way for AI models to connect with different data sources and external systems.

MCP solves a critical challenge in AI development: connecting AI assistants to external tools, data sources, and systems. Rather than building custom integrations for each combination of AI and external system, MCP provides a standardized protocol that works across platforms.

### Why MCP Matters

MCP dramatically expands what AI coding assistants like Cursor can do:

1. **Enhanced Access**: Connect your AI assistant to virtually any external system or data source
2. **Custom Behaviors**: Create specific actions tailored to your workflow
3. **Workflow Automation**: Allow your AI to handle more complex tasks automatically
4. **Standardization**: Use the same connectors across different AI tools

### Key Concepts in MCP

MCP architecture consists of several components:

- **MCP Hosts**: Applications running AI models, such as Claude Desktop or Cursor
- **MCP Clients**: Components that communicate with servers within Hosts
- **MCP Servers**: Core units providing tools, data, and context through standardized protocols
- **Local Data Sources**: Directly accessible data like local files and databases
- **Remote Services**: External APIs or services, such as GitHub or Slack

### Common Use Cases

MCP enables a variety of powerful capabilities:

1. **Debugging Tools**: Access console logs, monitor network requests, identify errors
2. **External Resource Access**: Connect to databases, APIs, or design tools
3. **Sequential Task Execution**: Structure complex operations for better workflow
4. **Content Generation**: Integrate with models like text-to-image generators

## Prerequisites and System Requirements

Before working with MCP servers, ensure your system meets these requirements:

### General Requirements

- **Operating System**: Windows, macOS, or Linux
- **Cursor IDE**: Latest version installed (check [cursor.sh](https://cursor.sh) for downloads)
- **Internet Connection**: Required for downloading MCPs and some server operations

### For TypeScript/JavaScript MCP Servers

- **Node.js**: Version 20.0 or higher
- **npm/yarn**: Latest version recommended
- **TypeScript**: Recommended for type safety (but optional)

Installation commands:
```bash
# Install Node.js and npm
# Visit https://nodejs.org/

# Verify installation
node --version
npm --version

# Install TypeScript globally (optional but recommended)
npm install -g typescript
```

### For Python MCP Servers

- **Python**: Version 3.10 or higher
- **pip or uv**: Package manager for Python
- **Virtual environment tool**: venv, conda, or uv

Installation commands:
```bash
# Install Python
# Visit https://www.python.org/

# Verify installation
python --version

# Install uv (recommended for MCP)
curl -sSf https://astral.sh/uv/install.sh | bash

# Verify uv installation
uv --version
```

### Optional Tools

- **Git**: For version control and cloning repositories
- **Chrome/Edge**: For browser-related MCPs
- **MCP Inspector**: For testing MCP servers
- **Cloud account**: For deploying MCPs (Cloudflare, AWS, etc.)

## Finding and Installing Existing MCPs

There are several marketplaces where you can find MCPs created by others:

### MCP Marketplaces

1. **Smithery.ai**: [https://smithery.ai](https://smithery.ai) - A registry of MCP servers
2. **Cursor Directory**: Built into Cursor
3. **MCP.so**: [https://mcp.so](https://mcp.so) - Community-created MCP resources
4. **GitHub MCP Repositories**:
   - Official: [https://github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)
   - Community: [https://github.com/smithery-ai/reference-servers](https://github.com/smithery-ai/reference-servers)

### Basic Steps to Add an MCP to Cursor

1. **Find an MCP**: Search for one that meets your needs on an MCP marketplace
2. **Copy the Command Line**: Each MCP will provide a command to use
3. **Add to Cursor**: 
   - Go to Cursor settings
   - Select "Features" 
   - Click on "MCP Server"
   - Click "Add MCP"
   - Give it a name
   - Choose the type (usually "Command")
   - Paste the command line
   - Click "Add"

### Using Smithery CLI (Optional)

For easier installation of MCPs from Smithery, you can use their CLI:

```bash
# Install a server (requires --client flag)
npx @smithery/cli install mcp-obsidian --client claude

# Install a server with pre-configured data (skips prompts)
npx @smithery/cli install mcp-obsidian --client claude --config '{"vaultPath":"path/to/vault"}'

# List available clients
npx @smithery/cli list clients

# Inspect a specific server from smithery's registry
npx @smithery/cli inspect mcp-obsidian
```

## Popular MCP Examples

Here are some popular and useful MCPs that can enhance your Cursor workflow:

### Sequential Thinking MCP

This MCP forces the agent to think through multiple steps and do planning.

**Installation:**
```bash
npx @smithery-ai/server-sequential-thinking
```

After installation, you can activate it in your prompts by including "using sequential thinking" or similar phrases.

### Browser Tools MCP

This powerful MCP gives Cursor access to your browser's console logs and network tabs for easier debugging.

**Installation Steps:**

1. Clone the Chrome plugin repository:
   ```bash
   git clone https://github.com/browser-tools/mcp-browser-tools
   ```

2. Load the extension in Chrome:
   - Go to Chrome settings → Extensions
   - Enable Developer Mode
   - Click "Load unpacked"
   - Choose the "Chrome extension" folder from the cloned repository

3. Add the MCP server to Cursor:
   - Go to Settings → Features → MCP Server
   - Add a new MCP called "Browser Tools"
   - Type: Command
   - Command: `npx @agentes/browser-tools-server`

4. Run the MCP server:
   ```bash
   npx @agentes/browser-tools-server
   ```

After setup, you can ask Cursor to check console logs, get network errors, and select DOM elements.

### Image Generation MCP

Here's how to create an MCP that generates images using the Replicate API:

1. Create a Cloudflare Worker project (instructions in the next section)
2. Add this code to your index.ts file:

```typescript
// Create an image generation function
server.tool(
  "generate_image",
  {
    prompt: z.string(),
    size: z.enum(["512x512", "768x768", "1024x1024"]).optional()
  },
  async ({ prompt, size = "512x512" }) => {
    // Add your Replicate API integration here
    // Example implementation placeholder
    const response = await fetch("https://api.replicate.com/v1/predictions", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "Authorization": `Token YOUR_REPLICATE_API_KEY`
      },
      body: JSON.stringify({
        version: "MODEL_VERSION_ID",
        input: {
          prompt: prompt,
          image_dimensions: size
        }
      })
    });
    
    // Handle response and return image URL
    // Implementation details here...
    
    return {
      content: [
        { 
          type: "text", 
          text: `Generated image URL: ${imageUrl}` 
        }
      ]
    };
  }
);
```

## Creating Your Own MCP Servers

You can create your own MCP servers to extend Cursor's capabilities in ways specific to your workflow.

### Basic Cloudflare Worker Setup

Cloudflare Workers provides a simple way to host MCP servers:

1. **Create a new project**:
   ```bash
   npm create cloudflare@latest
   ```

2. **Configure your project**:
   - Name your project (e.g., "my-mcp")
   - Choose "Hello World" example
   - Select TypeScript
   - Choose whether to use Git
   - Choose deployment options

3. **Navigate to your project folder**:
   ```bash
   cd my-mcp
   npm install
   ```

4. **Install MCP SDK**:
   ```bash
   npm install @modelcontextprotocol/sdk
   ```

5. **Create a basic MCP server in index.ts**:
   ```typescript
   import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
   import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
   import { z } from "zod";

   // Create an MCP server
   const server = new McpServer({ 
     name: "MyMcpServer", 
     version: "1.0.0" 
   });

   // Add a simple tool
   server.tool(
     "say_hello",
     { name: z.string() },
     async ({ name }) => ({
       content: [{ 
         type: "text", 
         text: `Hello, ${name}!` 
       }]
     })
   );

   // Start the server
   const transport = new StdioServerTransport();
   server.connect(transport).catch(error => {
     console.error("Server error:", error);
     process.exit(1);
   });
   ```

6. **Build your server**:
   ```bash
   npm run build
   ```

7. **Add your MCP to Cursor**:
   - Open Cursor settings
   - Go to Features → MCP Server
   - Add a new MCP server
   - Name it (e.g., "My MCP")
   - Type: Command
   - Command: `node path/to/my-mcp/dist/index.js`
   - Click Add

### TypeScript MCP Server

For a more complete TypeScript MCP server:

1. **Create a new project**:
   ```bash
   mkdir mcp-typescript-server
   cd mcp-typescript-server
   npm init -y
   ```

2. **Install dependencies**:
   ```bash
   npm install @modelcontextprotocol/sdk zod
   npm install --save-dev typescript @types/node
   ```

3. **Create tsconfig.json**:
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

4. **Create src/index.ts with a more advanced example**:
   ```typescript
   import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
   import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
   import { z } from "zod";

   // Create an MCP server
   const server = new McpServer({ 
     name: "AdvancedMcpServer", 
     version: "1.0.0" 
   });

   // Add a calculation tool
   server.tool(
     "calculate",
     { 
       operation: z.enum(["add", "subtract", "multiply", "divide"]),
       a: z.number(),
       b: z.number()
     },
     async ({ operation, a, b }) => {
       let result: number;
       
       switch (operation) {
         case "add":
           result = a + b;
           break;
         case "subtract":
           result = a - b;
           break;
         case "multiply":
           result = a * b;
           break;
         case "divide":
           if (b === 0) {
             throw new Error("Cannot divide by zero");
           }
           result = a / b;
           break;
       }
       
       return {
         content: [{ 
           type: "text", 
           text: `Result of ${operation} ${a} and ${b} is ${result}` 
         }]
       };
     }
   );

   // Start the server
   const transport = new StdioServerTransport();
   server.connect(transport).catch(error => {
     console.error("Server error:", error);
     process.exit(1);
   });
   ```

5. **Update package.json with build and start scripts**:
   ```json
   {
     "scripts": {
       "build": "tsc",
       "start": "node dist/index.js"
     },
     "type": "module"
   }
   ```

6. **Build and add to Cursor as described previously**

### Python MCP Server

For a Python-based MCP server:

1. **Create a new project**:
   ```bash
   mkdir mcp-python-server
   cd mcp-python-server
   ```

2. **Create and activate a virtual environment**:
   ```bash
   # Using uv (recommended)
   uv venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate

   # Or using traditional venv
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install the MCP SDK**:
   ```bash
   pip install mcp httpx
   ```

4. **Create server.py**:
   ```python
   from mcp.fastmcp import FastMCP, tool
   from typing import Literal

   # Create the MCP server
   app = FastMCP(
       name="PythonMcpServer",
       version="1.0.0"
   )

   @tool
   async def calculate(
       operation: Literal["add", "subtract", "multiply", "divide"],
       a: float,
       b: float
   ) -> str:
       """
       Perform a calculation on two numbers.
       
       Args:
           operation: The operation to perform
           a: First number
           b: Second number
           
       Returns:
           The result as a string
       """
       if operation == "add":
           result = a + b
       elif operation == "subtract":
           result = a - b
       elif operation == "multiply":
           result = a * b
       elif operation == "divide":
           if b == 0:
               raise ValueError("Cannot divide by zero")
           result = a / b
       else:
           raise ValueError(f"Unknown operation: {operation}")
           
       return f"Result of {operation} {a} and {b} is {result}"

   app.add_tool(calculate)

   if __name__ == "__main__":
       app.run()
   ```

5. **Add to Cursor**:
   - Open Cursor settings
   - Go to Features → MCP Server
   - Add a new MCP server
   - Name it (e.g., "Python MCP")
   - Type: Command
   - Command: `python path/to/mcp-python-server/server.py`
   - Click Add

## Best Practices for MCP Development

Follow these best practices when creating and using MCP servers:

### Server Design

1. **Follow the Single Responsibility Principle**: Each MCP server should focus on a specific domain or functionality.

2. **Implement Error Handling**: Gracefully handle errors and provide meaningful error messages.

3. **Secure Your Servers**: Be careful about who can access your MCP servers and what permissions they have.

4. **Provide Clear Documentation**: Document your server's tools, resources, and prompts thoroughly.

5. **Separate Configuration from Implementation**: Use environment variables or configuration files for API keys and other settings.

### Code Structure

1. **Modular Organization**: Organize code into modules for better maintainability.

2. **Type Safety**: Use TypeScript or Python type hints to ensure type safety.

3. **Clear Naming**: Use descriptive names for tools, resources, and prompts.

4. **Consistent Error Handling**: Establish a consistent pattern for error handling.

5. **Include Logging**: Implement proper logging to aid debugging.

### MCP Tool Design

1. **Clear Parameter Definitions**: Each tool should have clearly defined parameters with validation.

2. **Concise Descriptions**: Provide concise, helpful descriptions for each tool.

3. **Appropriate Scope**: Design tools with appropriate scope - not too broad or too narrow.

4. **Reasonable Defaults**: Provide sensible defaults for optional parameters.

5. **Response Format Consistency**: Maintain consistent response formats across tools.

## MCP Server Documentation Standards

Good documentation is crucial for MCP servers. Here's a recommended documentation structure:

### README.md Template

```markdown
# [Your MCP Server Name]

## Description
A brief description of what your MCP server does and what problems it solves.

## Features
- List key features and capabilities
- Each bullet point should be concise

## Prerequisites
- List all required software and dependencies
- Include version requirements if applicable

## Installation
Step-by-step installation instructions:

```bash
# Installation commands
npm install your-package
```

## Usage with Cursor
Instructions for adding to Cursor:

1. Open Cursor settings
2. Navigate to Features → MCP Server
3. Add new MCP with these details:
   - Name: [Suggested Name]
   - Type: Command
   - Command: `[Your Command]`

## Available Tools

### Tool 1: [Tool Name]
- **Description**: What this tool does
- **Parameters**:
  - `param1` (string): Description of parameter
  - `param2` (number): Description of parameter
- **Example Usage**: `Example of how to call this tool`

## Configuration
Details about any configuration options.

## Development
Instructions for developers who want to contribute or modify.

## License
License information.

## Contact
How to reach the maintainers.
```

## GitHub Repository Structure

A well-organized GitHub repository for your MCP server might look like this:

```
your-mcp-server/
├── .github/
│   └── workflows/            # CI/CD workflows
├── docs/                     # Documentation
│   ├── examples/             # Example usage
│   └── imgs/                 # Documentation images
├── src/                      # Source code
│   ├── tools/                # Tool implementations
│   ├── resources/            # Resource implementations
│   ├── prompts/              # Prompt implementations
│   ├── utils/                # Utility functions
│   ├── config.ts             # Configuration handling
│   └── index.ts              # Main entry point
├── tests/                    # Tests
├── .gitignore                # Git ignore file
├── LICENSE                   # License file
├── package.json              # NPM package file
├── README.md                 # Main documentation
└── tsconfig.json             # TypeScript configuration
```

## Testing and Debugging MCP Servers

### Testing Tools

1. **MCP Inspector**: A tool for testing MCP servers
2. **Cursor's Built-in Testing**: Test directly within Cursor
3. **Unit Tests**: Write unit tests for your MCP server

### Debugging Tips

1. **Enable Verbose Logging**: Add detailed logging to your server
2. **Check Claude's Logs**: Look for MCP-related errors in Claude's logs:
   ```bash
   tail -n 20 -f ~/Library/Logs/Claude/mcp*.log
   ```
3. **Verify Transport**: Make sure the transport method works correctly
4. **Test Tools Individually**: Test each tool in isolation

### Common Issues and Solutions

1. **Server Not Found**: Verify the path and command
2. **Tool Not Recognized**: Check tool registration and naming
3. **Permission Issues**: Verify file permissions
4. **API Rate Limits**: Implement rate limiting and error handling
5. **Data Format Errors**: Validate input and output formats

## MCP Resources and Community

### Official Resources

1. **Model Context Protocol Documentation**: [https://modelcontextprotocol.io](https://modelcontextprotocol.io)
2. **GitHub Repositories**:
   - [https://github.com/modelcontextprotocol](https://github.com/modelcontextprotocol)
   - [https://github.com/smithery-ai](https://github.com/smithery-ai)
3. **Anthropic MCP Introduction**: [https://www.anthropic.com/news/model-context-protocol](https://www.anthropic.com/news/model-context-protocol)

### Community Resources

1. **MCP Discord Server**: Join for discussions and help
2. **Smithery.ai Community**: [https://smithery.ai](https://smithery.ai)
3. **MCP.so Forum**: [https://mcp.so](https://mcp.so)

### Tutorials and Guides

1. **Egghead.io**: "Build Your First MCP Tool in Cursor in 2 Minutes"
2. **DataCamp Tutorial**: "Model Context Protocol (MCP): A Guide With Demo Project"
3. **Hackteam Guide**: "Build Your First MCP Server with TypeScript"

## Advanced Use Cases

### Database Integration

Connect your MCP server to databases:

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";
import sqlite3 from "sqlite3";
import { open } from "sqlite";

const server = new McpServer({ name: "DatabaseMCP", version: "1.0.0" });

// Database connection
let db;

async function initDb() {
  db = await open({
    filename: "./database.sqlite",
    driver: sqlite3.Database
  });
}

// Tool to query the database
server.tool(
  "query_database",
  { 
    query: z.string().describe("SQL query to execute (SELECT only)") 
  },
  async ({ query }) => {
    if (!query.toLowerCase().trim().startsWith("select")) {
      throw new Error("Only SELECT queries are allowed");
    }
    
    if (!db) {
      await initDb();
    }
    
    try {
      const results = await db.all(query);
      return {
        content: [{ 
          type: "text", 
          text: JSON.stringify(results, null, 2) 
        }]
      };
    } catch (error) {
      throw new Error(`Query error: ${error.message}`);
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

### AI Model Integration

Integrate your MCP server with another AI model:

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";
import OpenAI from "openai";

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY
});

const server = new McpServer({ name: "AIMcp", version: "1.0.0" });

server.tool(
  "generate_text",
  { 
    prompt: z.string(),
    max_tokens: z.number().min(10).max(1000).default(100)
  },
  async ({ prompt, max_tokens }) => {
    try {
      const response = await openai.completions.create({
        model: "gpt-3.5-turbo-instruct",
        prompt,
        max_tokens
      });
      
      return {
        content: [{ 
          type: "text", 
          text: response.choices[0].text.trim() 
        }]
      };
    } catch (error) {
      throw new Error(`OpenAI API error: ${error.message}`);
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

## Appendix: Installation Guides

### Installing Node.js

#### Windows
1. Visit [https://nodejs.org/](https://nodejs.org/)
2. Download the LTS version
3. Run the installer and follow instructions
4. Verify installation by opening Command Prompt and typing:
   ```
   node --version
   npm --version
   ```

#### macOS
1. Using Homebrew (recommended):
   ```bash
   brew install node
   ```
2. Or download from [https://nodejs.org/](https://nodejs.org/)
3. Verify installation:
   ```bash
   node --version
   npm --version
   ```

#### Linux (Ubuntu/Debian)
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
node --version
npm --version
```

### Installing Python and uv

#### Windows
1. Download Python from [https://www.python.org/downloads/](https://www.python.org/downloads/)
2. Run the installer (check "Add Python to PATH")
3. Install uv:
   ```bash
   curl -sSf https://astral.sh/uv/install.ps1 | powershell
   ```
4. Restart your terminal and verify:
   ```bash
   python --version
   uv --version
   ```

#### macOS
1. Using Homebrew:
   ```bash
   brew install python
   curl -sSf https://astral.sh/uv/install.sh | bash
   ```
2. Verify installation:
   ```bash
   python3 --version
   uv --version
   ```

#### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install python3 python3-pip
curl -sSf https://astral.sh/uv/install.sh | bash
python3 --version
uv --version
```

### Installing Git

#### Windows
1. Download Git from [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Run the installer
3. Verify installation:
   ```bash
   git --version
   ```

#### macOS
```bash
brew install git
git --version
```

#### Linux (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install git
git --version
```
