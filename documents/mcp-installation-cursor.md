## Standard MCP Configuration Template

When adding MCP servers to your Cursor configuration, you'll typically follow a standard template pattern. Here's the general structure:

```json
{
  "mcpServers": {
    "your-server-name": {
      "command": "cmd",                           // On Windows
      "args": [
        "/c",                                     // On Windows
        "npx",
        "-y",
        "@smithery/cli@latest",
        "run",
        "@smithery-ai/server-name",               // Replace with actual server name
        "--config",
        "{\"paramName\":\"paramValue\"}"          // Replace with actual config parameters
      ]
    }
  }
}
```

### Key Components:

1. **Server Name** (`"your-server-name"`): 
   - A descriptive name you choose to identify the MCP server
   - Example: `"github"`, `"sequential-thinking"`, `"filesystem"`

2. **Command** (`"command"`):
   - On Windows: `"cmd"`
   - On macOS/Linux: typically `"npx"`

3. **Arguments** (`"args"`):
   - Command prefix (Windows only): `"/c"`
   - Package manager: `"npx"`
   - Installation flag: `"-y"`
   - Smithery CLI: `"@smithery/cli@latest"`
   - Command: `"run"`
   - Server package: `"@smithery-ai/server-name"` (replace with actual server name)
   - Configuration flag: `"--config"`
   - Configuration JSON: `"{\"paramName\":\"paramValue\"}"` (with escaped quotes)

4. **Configuration Parameters**:
   - Simple servers (like Sequential Thinking): `"{}"`
   - GitHub: `"{\"githubPersonalAccessToken\":\"your_token_here\"}"`
   - Database servers: `"{\"connectionString\":\"your_connection_string\"}"`
   - Custom parameters: Depends on the specific MCP server's requirements

### Example Configurations:

**Sequential Thinking (Windows)**:
```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@smithery/cli@latest",
        "run",
        "@smithery-ai/server-sequential-thinking",
        "--config",
        "{}"
      ]
    }
  }
}
```

**Sequential Thinking (macOS/Linux)**:
```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": [
        "-y",
        "@smithery/cli@latest",
        "run",
        "@smithery-ai/server-sequential-thinking",
        "--config",
        "{}"
      ]
    }
  }
}
```

**GitHub (Windows)**:
```json
{
  "mcpServers": {
    "github": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@smithery/cli@latest",
        "run",
        "@smithery-ai/github",
        "--config",
        "{\"githubPersonalAccessToken\":\"your_github_token_here\"}"
      ]
    }
  }
}
```

**GitHub (macOS/Linux)**:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "@smithery/cli@latest",
        "run",
        "@smithery-ai/github",
        "--config",
        "{\"githubPersonalAccessToken\":\"your_github_token_here\"}"
      ]
    }
  }
}
```
```

The consistent structure makes it easier to add new MCP servers once you understand the pattern. Simply replace the server name and configuration parameters as needed for each specific tool.## Using MCP with Cursor

### Mode Selection Requirements

When using MCP tools in Cursor, you must select the correct interaction mode:

1. **Agent Mode**: Required for using MCP tools
   - Located in the sidebar dropdown menu (marked with ∞ symbol)
   - Access with keyboard shortcut: Ctrl+L
   - This mode enables Cursor to use tools, including MCP tools

2. **Other Modes**:
   - **Ask Mode**: Simple chat without tool usage capabilities
   - **Edit Mode**: For direct code editing without conversation

![Cursor Mode Selection](https://example.com/cursor-mode-selection.png)

**Important**: If you don't specifically select Agent mode, MCP tools will not be available. Even if your MCP server configuration is correct, Cursor must be in Agent mode to utilize these tools.

### Interaction Flow

When in Agent mode with properly configured MCP servers:

1. Type your query (e.g., "What are the GitHub repositories for [your-github-username]?")
2. Cursor will identify an appropriate MCP tool for the task
3. You'll be prompted to approve or deny the tool usage
4. Upon approval, the tool will execute and results will be incorporated into the response### Appendix F: Command Cheat Sheet

#### Node.js Commands
- Check version: `node --version`
- Run script: `node script.js`
- Install package: `npm install package-name`
- Run npx package: `npx package-name`

#### Python Commands
- Check version: `python --version` or `python3 --version`
- Run script: `python script.py` or `python3 script.py`
- Install package: `pip install package-name` or `uv pip install package-name`
- Create venv: `python -m venv venv` or `uv venv`

#### Git Commands
- Clone repository: `git clone https://github.com/username/repo.git`
- Pull updates: `git pull`
- Check status: `git status`

#### Cursor MCP Commands
- Add MCP server: Settings → Features → MCP Server → Add MCP Server
- Check server status: Green dot = running, Red dot = error
- Force restart MCP: Restart Cursor# MCP Installation Guide for Cursor

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation on Windows](#installation-on-windows)
- [Installation on macOS](#installation-on-macos)
- [Installation on Linux](#installation-on-linux)
- [Setting Up MCP in Cursor](#setting-up-mcp-in-cursor)
- [Popular MCP Servers for Cursor](#popular-mcp-servers-for-cursor)
- [Verification and Testing](#verification-and-testing)
- [Troubleshooting](#troubleshooting)
- [Appendix: Detailed Installation Guides](#appendix-detailed-installation-guides)

## Introduction

The Model Context Protocol (MCP) extends Cursor's AI capabilities by connecting it to external data sources, tools, and services. This guide walks you through setting up MCP servers for Cursor across different operating systems.

## Prerequisites

Before installing MCP servers for Cursor, ensure you have:

1. **Cursor**: Download and install from [cursor.sh](https://cursor.sh)
2. **Operating System Requirements**:
   - Windows 10/11 (64-bit)
   - macOS 11 (Big Sur) or later
   - Ubuntu 20.04 or later (other Linux distributions may work but aren't officially supported)
3. **Basic Tools**:
   - Terminal/Command Prompt access
   - Administrative privileges (for some installations)

### Verifying Prerequisites

You can check if you already have the required tools installed by using Cursor's built-in terminal:

1. In Cursor, go to Terminal → New Terminal
2. Check for Node.js and npm:
   ```bash
   node --version
   npm --version
   ```
3. Check for Python:
   ```bash
   python --version
   # or on some systems
   python3 --version
   ```
4. Check for Git:
   ```bash
   git --version
   ```

If these commands return version numbers (like v20.17.0 for Node.js or 3.12.6 for Python), you already have the prerequisites installed and can skip to [Setting Up MCP in Cursor](#setting-up-mcp-in-cursor).

## Installation on Windows

### Step 1: Install Node.js and npm

1. Visit [Node.js download page](https://nodejs.org/)
2. Download the LTS version (recommended for most users)
3. Run the installer, keeping default options
4. Verify installation by opening Command Prompt and typing:
   ```
   node --version
   npm --version
   ```

### Step 2: Install Python (Required for Some MCP Servers)

1. Visit [Python download page](https://www.python.org/downloads/windows/)
2. Download the latest stable release (3.10+ recommended)
3. During installation, check "Add Python to PATH"
4. Verify installation:
   ```
   python --version
   ```

### Step 3: Install Git (For Cloning MCP Repositories)

1. Download Git from [git-scm.com](https://git-scm.com/download/win)
2. Run the installer with default options
3. Verify installation:
   ```
   git --version
   ```

## Installation on macOS

### Step 1: Install Homebrew (Package Manager)

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Step 2: Install Node.js and npm

```bash
brew install node
```

Verify installation:
```bash
node --version
npm --version
```

### Step 3: Install Python

```bash
brew install python
```

Verify installation:
```bash
python3 --version
```

### Step 4: Install Git

```bash
brew install git
```

## Installation on Linux

### Ubuntu/Debian

#### Step 1: Install Node.js and npm

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Verify installation:
```bash
node --version
npm --version
```

#### Step 2: Install Python 3

```bash
sudo apt update
sudo apt install python3 python3-pip
```

Verify installation:
```bash
python3 --version
pip3 --version
```

#### Step 3: Install Git

```bash
sudo apt update
sudo apt install git
```

### Fedora/RHEL/CentOS

#### Step 1: Install Node.js and npm

```bash
sudo dnf install nodejs
```

#### Step 2: Install Python 3

```bash
sudo dnf install python3 python3-pip
```

#### Step 3: Install Git

```bash
sudo dnf install git
```

## Setting Up MCP in Cursor

Cursor provides a JSON-based configuration approach for setting up MCP servers:

### JSON Configuration Method (Current Method)

1. Open Cursor
2. Click on the settings icon (⚙️) in the bottom left
3. Select "Features" from the menu
4. Find "MCP Server" in the features list
5. Click "Add new global MCP server" button

This will open the `mcp.json` file (located at `C:\Users\[username]\.cursor\mcp.json` on Windows or `~/.cursor/mcp.json` on macOS/Linux).

Edit this file with your MCP server configurations following this structure:

```json
{
  "mcpServers": {
    "server-name": {
      "command": "command-to-run",
      "args": [
        "argument1",
        "argument2"
      ],
      "env": {
        "ENV_VAR1": "value1"
      }
    }
  }
}
```

Save the file and restart Cursor to apply the changes.

## Popular MCP Servers for Cursor

Here's how to set up some popular MCP servers in Cursor using the JSON configuration method:

### GitHub MCP (Using Classic Token)

For GitHub integration with Cursor, use a Classic token rather than a Fine-grained token for better compatibility and functionality.

1. First create a GitHub Classic token:
   - Go to GitHub → Settings → Developer Settings → Personal Access Tokens → Tokens (classic)
   - Click "Generate new token" → "Generate new token (classic)"
   - Give it a descriptive name
   - Set an expiration period
   - Select these scopes:
     - `repo` (Full control of private repositories)
     - `workflow` (if you need GitHub Actions)
     - `read:org` (if you need organization access)
   - Click "Generate token" and save the token value

2. Edit your `mcp.json` file to add the GitHub MCP:

   #### Windows:
   ```json
   {
     "mcpServers": {
       "github": {
         "command": "cmd",
         "args": [
           "/c",
           "npx",
           "-y",
           "@smithery/cli@latest",
           "run",
           "@smithery-ai/github",
           "--config",
           "{\"githubPersonalAccessToken\":\"your_github_token_here\"}"
         ]
       }
     }
   }
   ```

   #### macOS/Linux:
   ```json
   {
     "mcpServers": {
       "github": {
         "command": "npx",
         "args": [
           "-y",
           "@smithery/cli@latest",
           "run",
           "@smithery-ai/github",
           "--config",
           "{\"githubPersonalAccessToken\":\"your_github_token_here\"}"
         ]
       }
     }
   }
   ```

3. Replace `your_github_token_here` with your actual GitHub classic token
4. Save the file and restart Cursor

### Filesystem MCP

This MCP gives Cursor access to your filesystem:

1. Edit your `mcp.json` file to add the Filesystem MCP:
   ```json
   {
     "mcpServers": {
       "filesystem": {
         "command": "npx",
         "args": [
           "@modelcontextprotocol/filesystem"
         ]
       }
     }
   }
   ```

2. Save the file and restart Cursor

### Brave Search MCP

This MCP enables web search capabilities:

1. Get a Brave Search API key from [https://brave.com/search/api/](https://brave.com/search/api/)

2. Edit your `mcp.json` file to add the Brave Search MCP:
   ```json
   {
     "mcpServers": {
       "brave-search": {
         "command": "npx",
         "args": [
           "@modelcontextprotocol/brave-search",
           "--api-key",
           "YOUR_API_KEY"
         ]
       }
     }
   }
   ```

3. Replace `YOUR_API_KEY` with your actual Brave Search API key
4. Save the file and restart Cursor

### Multiple MCP Servers

### Adding Additional Servers to Existing Configuration

When you already have one MCP server configured (like GitHub) and want to add another one, you need to:

1. Add the new server configuration **inside** the existing `"mcpServers"` object
2. Separate each server entry with a comma
3. Maintain the overall JSON structure

Here's how to add a Sequential Thinking MCP server to an existing GitHub configuration:

**Before (only GitHub):**
```json
{
  "mcpServers": {
    "github": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@smithery/cli@latest",
        "run",
        "@smithery-ai/github",
        "--config",
        "{\"githubPersonalAccessToken\":\"your_github_token_here\"}"
      ]
    }
  }
}
```

**After (GitHub + Sequential Thinking):**
```json
{
  "mcpServers": {
    "github": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@smithery/cli@latest",
        "run",
        "@smithery-ai/github",
        "--config",
        "{\"githubPersonalAccessToken\":\"your_github_token_here\"}"
      ]
    },
    "sequential-thinking": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@smithery/cli@latest",
        "run",
        "@smithery-ai/server-sequential-thinking",
        "--config",
        "{}"
      ]
    }
  }
}
```

**Important Notes:**
- Don't forget the comma after the closing brace `}` of the first MCP server
- Make sure the new entry is inside the `"mcpServers"` object (within its curly braces)
- Each server needs a unique name (like "github" and "sequential-thinking")
- The overall structure has only one opening `{` at the beginning and one closing `}` at the end
```

## Verification and Testing

After configuring MCP servers:

1. Restart Cursor
2. Go to Settings → Features → MCP Server to check the status
   - Green dot: Server is running correctly
   - Red dot: Server has issues or is not running
3. Open a new chat in Cursor's Agent mode
4. Test the server with a relevant prompt:
   - For Sequential Thinking: "Plan a project using sequential thinking"
   - For Browser Tools: "Check for console errors on the current page"
   - For GitHub: "What are the GitHub repositories for [your-github-username]?" (must specify the username)

When using an MCP tool, Cursor will typically:
1. Show that it's using the tool
2. Ask for your permission
3. Execute the tool and incorporate the results

### Important Note for GitHub MCP

When using the GitHub MCP, you must specify the GitHub username in your queries. For example:
- "What are the GitHub repositories for username123?"
- "Show me the README from username123's project-name repository"
- "List the open issues for username123's repository project-name"

This username specificity is required for the GitHub MCP to correctly target the right account.

## Using MCP with Cursor

## Troubleshooting

### Common Issues and Solutions

#### JSON Syntax Errors
- Make sure your JSON is properly formatted with correct quotes, commas, and brackets
- Check for missing or extra commas, especially when adding multiple servers
- Ensure that special characters in tokens are properly escaped (especially quotes with `\"`)
- Verify Windows-specific configurations use `"command": "cmd"` and `"/c"` in the args array
- Use a JSON validator to check your syntax if you encounter errors
- Remember to remove the GitHub token from examples before sharing configurations

#### Server Not Showing in Cursor
- Make sure you saved the `mcp.json` file
- Verify the file is in the correct location: `~/.cursor/mcp.json` (macOS/Linux) or `C:\Users\[username]\.cursor\mcp.json` (Windows)
- Restart Cursor completely
- Check if the command is correctly specified
- Verify the package is installed (try running the command in a terminal)

#### Server Showing Red Status
- The server might be offline or encountering errors
- Check command spelling and path correctness
- Try running the server manually to see error messages
- For GitHub, verify your token is valid and has the correct permissions

#### Permission Issues
- Some MCPs require appropriate permissions (e.g., filesystem access)
- Check file permissions
- Try running with appropriate privileges

#### Connection Issues
- Restart Cursor
- Check your network connection
- Verify firewall settings aren't blocking connections

### Debug Logging

For troubleshooting MCP servers:

1. Run the server manually in a terminal to see output
2. Check for log files (location varies by MCP server)
3. Use the `--verbose` flag if available:
   ```bash
   npx @server-name/package --verbose
   ```

## Appendix: Detailed Installation Guides

### Appendix A: Installing uv (Python Package Manager)

The `uv` tool is a faster alternative to pip for Python package management.

#### Windows
```
curl -sSf https://astral.sh/uv/install.ps1 | powershell
```

#### macOS/Linux
```
curl -sSf https://astral.sh/uv/install.sh | bash
```

Verify installation:
```
uv --version
```

Using uv to install packages:
```
uv pip install mcp httpx
```

### Appendix B: Creating a Python Virtual Environment

Using virtual environments is recommended for Python MCP servers.

#### With uv (Recommended)
```bash
# Create a new directory
mkdir my-mcp-server
cd my-mcp-server

# Create and activate a virtual environment
uv venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate

# Install MCP packages
uv pip install mcp httpx
```

#### With venv (Standard Library)
```bash
# Create a new directory
mkdir my-mcp-server
cd my-mcp-server

# Create and activate a virtual environment
python -m venv venv
# Windows
venv\Scripts\activate
# macOS/Linux
source venv/bin/activate

# Install MCP packages
pip install mcp httpx
```

### Appendix C: Using Cursor's Terminal for Installation

Cursor's built-in terminal provides a convenient way to install and manage packages without switching between applications.

#### Terminal Access

1. Open Cursor
2. Go to Terminal → New Terminal (or use the shortcut Ctrl+` on Windows/Linux, or Cmd+` on macOS)
3. The terminal defaults to your current working directory or project folder

#### Directory Considerations

When working with npm and Python packages in Cursor's terminal:

1. **Node.js/npm Installation**:
   - The Node.js installer itself is installed system-wide regardless of the current directory
   - When using `npm install -g package-name` (with the `-g` flag), packages are installed globally and available from any directory
   - When using `npm install package-name` (without the `-g` flag), packages are installed locally in the current directory's `node_modules` folder

2. **Python Package Installation**:
   - When using `pip install package-name`, the package is installed either:
     - System-wide if not using a virtual environment
     - In the active virtual environment if one is activated
   - Always check which Python environment is active with `which python` (Linux/macOS) or `where python` (Windows)

3. **Project Organization**:
   - It's recommended to create dedicated folders for your MCP server projects
   - Use virtual environments for Python-based MCP servers
   - Initialize Node.js projects with `npm init -y` for JavaScript/TypeScript-based MCP servers

#### Command Execution

When adding MCP servers to Cursor, you can use either:
- Relative paths: `node ./path/to/server.js` (relative to Cursor's working directory)
- Absolute paths: `node C:/full/path/to/server.js` (recommended for reliability)
- Global packages: `npx package-name` (if installed with `-g` flag)

### Appendix D: JavaScript Package Management

#### Using npm
```bash
# Initialize a new project
mkdir my-mcp-server
cd my-mcp-server
npm init -y

# Install MCP SDK
npm install @modelcontextprotocol/sdk
```

#### Using yarn
```bash
# Initialize a new project
mkdir my-mcp-server
cd my-mcp-server
yarn init -y

# Install MCP SDK
yarn add @modelcontextprotocol/sdk
```

### Appendix E: Using Smithery CLI

Smithery provides a command-line tool for easier MCP server management:

```bash
# Install the Smithery CLI
npm install -g @smithery/cli

# List available MCP servers
npx @smithery/cli list servers

# Install a server for Cursor
npx @smithery/cli install <server-name> --client cursor

# Inspect server capabilities
npx @smithery/cli inspect <server-name>
```