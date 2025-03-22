# MCP Installation Guide for Claude

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation on Windows](#installation-on-windows)
- [Installation on macOS](#installation-on-macos)
- [Installation on Linux](#installation-on-linux)
- [Setting Up Claude Desktop](#setting-up-claude-desktop)
- [Configuring MCP Servers in Claude](#configuring-mcp-servers-in-claude)
- [Verification and Testing](#verification-and-testing)
- [Troubleshooting](#troubleshooting)
- [Appendix: Detailed Installation Guides](#appendix-detailed-installation-guides)

## Introduction

The Model Context Protocol (MCP) allows Claude to connect to external data sources and tools, enhancing its capabilities. This guide will walk you through setting up MCP servers for Claude Desktop across different operating systems.

## Prerequisites

Before installing MCP servers for Claude, ensure you have:

1. **Claude Desktop**: Download and install from [Anthropic's website](https://www.anthropic.com/claude)
2. **Operating System Requirements**:
   - Windows 10/11 (64-bit)
   - macOS 11 (Big Sur) or later
   - Ubuntu 20.04 or later (other Linux distributions may work but are not officially supported)
3. **Basic Tools**:
   - Terminal/Command Prompt access
   - Administrative privileges (for some installations)

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

## Setting Up Claude Desktop

1. Download Claude Desktop from [Anthropic's website](https://www.anthropic.com/claude)
2. Install and open the application
3. Sign in with your Anthropic account
4. Navigate to Settings > Preferences (or similar, depending on your version)

## Configuring MCP Servers in Claude

Claude Desktop uses a JSON configuration file to set up MCP servers. Here's how to configure it:

### Step 1: Locate the Configuration File

#### Windows
```
%APPDATA%\Claude\claude_desktop_config.json
```

#### macOS
```
~/Library/Application Support/Claude/claude_desktop_config.json
```

#### Linux
```
~/.config/Claude/claude_desktop_config.json
```

### Step 2: Edit the Configuration File

If the file doesn't exist, create it with the following structure:

```json
{
  "mcpServers": {
    "server-name": {
      "command": "node",
      "args": [
        "path/to/mcp/server.js"
      ]
    }
  }
}
```

For Python-based servers:

```json
{
  "mcpServers": {
    "python-server": {
      "command": "python",
      "args": [
        "path/to/mcp/server.py"
      ]
    }
  }
}
```

### Step 3: Adding Common MCP Servers

Here are examples for adding popular MCP servers:

#### Sequential Thinking MCP

```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": [
        "@smithery-ai/server-sequential-thinking"
      ]
    }
  }
}
```

#### Filesystem MCP

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

#### Multiple Servers Example

You can add multiple servers simultaneously:

```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": [
        "@smithery-ai/server-sequential-thinking"
      ]
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "@modelcontextprotocol/filesystem"
      ]
    },
    "weather": {
      "command": "python",
      "args": [
        "/path/to/weather-server.py"
      ]
    }
  }
}
```

## Verification and Testing

After configuring MCP servers:

1. Restart Claude Desktop
2. Start a new chat
3. Look for MCP indicators in the UI (e.g., a tool icon or indicator)
4. Test a server with a simple prompt, such as:
   - For Sequential Thinking: "Help me plan a project using sequential thinking"
   - For Filesystem: "Can you list the files in my Downloads folder?"

## Troubleshooting

### Common Issues and Solutions

#### Server Not Found
- Verify the path in your configuration
- Check for typos in command or package names
- Make sure packages are installed globally if using npx

#### Permission Errors
- Use absolute paths in configuration
- Check file access permissions
- Try running with elevated privileges

#### MCP Server Not Starting
- Check Claude's logs for more information:
  ```
  tail -n 50 -f ~/Library/Logs/Claude/mcp*.log  # macOS/Linux
  ```
- Verify Node.js or Python version compatibility
- Try running the server manually from the command line first

#### No Tools Available
- Ensure the server is properly implemented with tool definitions
- Restart Claude Desktop after configuration changes
- Check that server initialization completes without errors

### Logging and Debugging

To see logs from Claude's MCP servers:

#### Windows
```
type %APPDATA%\Claude\Logs\mcp-server-*.log
```

#### macOS
```
cat ~/Library/Logs/Claude/mcp-server-*.log
```

#### Linux
```
cat ~/.config/Claude/Logs/mcp-server-*.log
```

## Appendix: Detailed Installation Guides

### Appendix A: Installing uv (Python Package Manager)

The `uv` tool is a faster alternative to pip for Python package management. Many MCP servers use it for better performance.

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

### Appendix C: JavaScript Package Management

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

### Appendix D: Command Cheat Sheet

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

### Appendix E: Using Smithery CLI

Smithery provides a command-line tool for easier MCP server management:

```bash
# Install the Smithery CLI
npm install -g @smithery/cli

# List available MCP servers
npx @smithery/cli list servers

# Install a server for Claude
npx @smithery/cli install <server-name> --client claude

# Inspect server capabilities
npx @smithery/cli inspect <server-name>
```