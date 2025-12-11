# Claude Code Setup With Qwen

This comprehensive guide will walk you through setting up Claude Code with Qwen models, including detailed explanations for each command and step.

---

## 1. Overview

This guide will help you set up Claude Code to work with Alibaba's Qwen models. Claude Code acts as a command-line interface that can be configured to use different AI models through a routing mechanism. This setup allows you to use Claude Code commands with the power of Qwen models for free.

## 2. Prerequisites

Before beginning, make sure the following are installed:

* **Qwen CLI** (installed and authenticated)
* **Node.js v18 or later**

### Verify Node.js Installation

Open PowerShell and run:

```powershell
node --version
npm --version
```

- If the version is 18.x or higher for Node.js, you're good to continue.
- If not, download and install the latest LTS release from [https://nodejs.org](https://nodejs.org)

## 3. Project Files & Locations

The setup will create these configuration files in your user directory:
- `~/.claude-code-router/config.json` - Router configuration file
- `~/.claude/settings.local.json` - Claude Code settings (if needed)
- `~/.qwen/oauth_creds.json` - Qwen authentication credentials

## 4. Step-by-Step Setup Instructions

### Step 1: Install Qwen CLI

The Qwen CLI is required to authenticate and manage your Qwen account.

#### Command Explanation:
```bash
npm install -g @qwen-code/qwen-code@latest
```

- `npm`: Node Package Manager - a package manager for JavaScript
- `install`: Command to install packages
- `-g`: Global flag - installs the package globally so it's available system-wide
- `@qwen-code/qwen-code@latest`: Package name with the latest version tag

Run this command in PowerShell:

```bash
npm install -g @qwen-code/qwen-code@latest
```

#### Verify the installation:

```bash
qwen --version
```

- This command checks if Qwen CLI is properly installed and shows the version number.

#### Authenticate with Qwen:

```bash
qwen
```

- This command will start the authentication process with Qwen. Follow the prompts to log in to your Qwen account.

### Step 2: Install Claude Code and Router

Install Claude Code and the router globally to enable routing requests to Qwen models.

#### Command Explanation:
```bash
npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router
```

- `@anthropic-ai/claude-code`: The Claude Code CLI tool
- `@musistudio/claude-code-router`: A router that forwards Claude Code requests to different AI models (in this case, Qwen)
- Both packages are installed globally (`-g`) so they're available system-wide

Run this command in PowerShell:

```bash
npm install -g @anthropic-ai/claude-code @musistudio/claude-code-router
```

#### Verify installations:

```bash
claude --version
ccr version
```

- `claude --version`: Checks if Claude Code CLI is properly installed
- `ccr version`: Checks if Claude Code Router is properly installed
- `ccr` is the shorthand command for Claude Code Router

### Step 3: Get Your Qwen Access Token

You need to locate your Qwen access token to configure the router.

#### Command Explanation:
The token is stored in a hidden file in your user directory:
```
C:\Users\PC_USER\.qwen\oauth_creds.json
```

#### Steps to locate your token:

1. Open a new terminal
2. Type `code .` to open the current directory in VS Code (optional, for file navigation)
3. Navigate to your user directory to find the `.qwen` folder
4. Open the `oauth_creds.json` file in the `.qwen` folder

You will see something like this:

```json
{
  "access_token": "YOUR_QWEN_ACCESS_TOKEN_HERE",
  "token_type": "Bearer",
  "refresh_token": "YOUR_QWEN_REFRESH_TOKEN_HERE",
  "resource_url": "portal.qwen.ai",
  "expiry_date": 1764876220290
}
```

5. Copy the value from `"access_token": "..."` - you'll need this for the router configuration.

**Purpose**: The access token authenticates your requests to the Qwen API.

### Step 4: Create Required Configuration Folders

Create the necessary directories for Claude Code and the router configuration.

#### Command Explanation:
```bash
mkdir -p ~/.claude-code-router ~/.claude
```

- `mkdir`: Make directory command
- `-p`: Creates parent directories as needed and doesn't error if directories already exist
- `~/.claude-code-router`: Directory for router configuration files
- `~/.claude`: Directory for Claude Code configuration files
- `~` represents your home directory (e.g., `C:\Users\YourUsername\` on Windows)

On Windows PowerShell, the equivalent command is:

```powershell
New-Item -ItemType Directory -Path "$env:USERPROFILE\.claude-code-router" -Force
New-Item -ItemType Directory -Path "$env:USERPROFILE\.claude" -Force
```

**Purpose**: These directories store configuration files needed for Claude Code and the router to function properly.

### Step 5: Create the Router Configuration

Create the configuration file that tells the router how to connect to Qwen models.

#### Command Explanation for Linux/Mac:
```bash
cat > ~/.claude-code-router/config.json << 'EOF'
{
  "LOG": true,
  "LOG_LEVEL": "info",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "API_TIMEOUT_MS": 600000,
  "Providers": [
    {
      "name": "qwen",
      "api_base_url": "https://portal.qwen.ai/v1/chat/completions",
      "api_key": "YOUR_QWEN_ACCESS_TOKEN_HERE",
      "models": [
        "qwen3-coder-plus",
        "qwen3-coder-max",
        "qwen3-coder-turbo"
      ]
    }
  ],
  "Router": {
    "default": "qwen,qwen3-coder-plus",
    "background": "qwen,qwen3-coder-plus",
    "think": "qwen,qwen3-coder-plus",
    "longContext": "qwen,qwen3-coder-plus",
    "longContextThreshold": 60000,
    "webSearch": "qwen,qwen3-coder-plus"
  }
}
EOF
```

- `cat >`: Redirects input to a file
- `<< 'EOF'`: Starts a here-document that ends when it encounters the EOF marker
- This creates a JSON configuration file with settings for the Qwen provider

#### For Windows PowerShell users:
Since Windows doesn't support the `cat << EOF` syntax, create the file using Notepad:

```powershell
notepad "$env:USERPROFILE\.claude-code-router\config.json"
```

Then paste this JSON configuration:

```json
{
  "LOG": true,
  "LOG_LEVEL": "info",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "API_TIMEOUT_MS": 600000,
  "Providers": [
    {
      "name": "qwen",
      "api_base_url": "https://portal.qwen.ai/v1/chat/completions",
      "api_key": "YOUR_QWEN_ACCESS_TOKEN_HERE",
      "models": [
        "qwen3-coder-plus",
        "qwen3-coder-max",
        "qwen3-coder-turbo"
      ]
    }
  ],
  "Router": {
    "default": "qwen,qwen3-coder-plus",
    "background": "qwen,qwen3-coder-plus",
    "think": "qwen,qwen3-coder-plus",
    "longContext": "qwen,qwen3-coder-plus",
    "longContextThreshold": 60000,
    "webSearch": "qwen,qwen3-coder-plus"
  }
}
```

**Replace `YOUR_QWEN_ACCESS_TOKEN_HERE`** with your actual access token from Step 3.

#### Configuration Explanation:

- `"LOG": true` - Enables logging for debugging
- `"LOG_LEVEL": "info"` - Sets logging level to show informational messages
- `"HOST": "127.0.0.1"` - Localhost address where the router will run
- `"PORT": 3456` - Port number where the router will listen for requests
- `"API_TIMEOUT_MS": 600000` - Timeout set to 10 minutes (600,000ms) for long operations
- `"Providers"` - Defines the AI model providers (in this case, Qwen)
- `"Router"` - Maps different Claude Code operations to specific models

#### Router Mapping Explanations:
- `"default"`: Used for regular Claude Code commands
- `"background"`: Used for background tasks
- `"think"`: Used for thinking/analysis tasks
- `"longContext"`: Used for tasks requiring long context
- `"webSearch"`: Used for web search functionality

## 5. Usage in This Project

### Start the Router

Start the Claude Code router to begin forwarding requests to Qwen.

```bash
ccr restart
```

- `ccr`: Claude Code Router command
- `restart`: Stops any existing router instance and starts a new one
- This ensures the router is using your newly created configuration

Run this command in PowerShell:

```bash
ccr restart
```

Wait until you see:
```
✔ Service started successfully
```

**Purpose**: The router acts as a bridge between Claude Code and the Qwen API, translating Claude Code commands into Qwen-compatible requests.

### Quick Test

Test your Claude Code setup with Qwen models.

```bash
ccr code
```

- `ccr code`: Launches Claude Code through the router
- This command starts an interactive Claude Code session using Qwen models

Run this command in PowerShell:

```bash
ccr code
```

When the Claude interface opens, try:

```
> hi
```

**Purpose**: This tests if Claude Code is properly communicating with the Qwen models through the router.

## 6. Troubleshooting

### Handling 401 Token Errors (Authentication Issues)

If your Qwen OAuth token expires, you'll need to refresh it.

#### 1️⃣ Reauthenticate
If tokens don't match or you get authentication errors, delete the old credentials and reauthenticate:

```bash
# On Windows, navigate to the .qwen folder and delete oauth_creds.json
Remove-Item "$env:USERPROFILE\.qwen\oauth_creds.json"
```

Then re-run the authentication:

```bash
qwen
```

#### 2️⃣ Update your router config
If you get a new token, update your router configuration:

```powershell
notepad "$env:USERPROFILE\.claude-code-router\config.json"
```

Replace the `api_key` value with your new `access_token`.

#### 3️⃣ Restart the router
```bash
ccr restart
```

**Purpose**: Token expiration is common with OAuth-based APIs. This process ensures you maintain access to the Qwen API.

### Router Issues
If the router won't start:

1. Check if port 3456 is available:
```powershell
Test-NetConnection -ComputerName localhost -Port 3456
```

2. Restart the router:
```powershell
ccr restart
```

### Connection Issues
If Claude Code can't connect:

1. Verify the router is running:
```powershell
ccr status
```

2. Check your internet connection and firewall settings

## 7. Notes & Best Practices

### Important Notes

1. **Replace `YOUR_QWEN_ACCESS_TOKEN_HERE`** with your actual access token from the oauth_creds.json file
2. **The configuration maps different Claude Code operations** to appropriate Qwen models
3. **Make sure your Qwen account has sufficient credits/usage allowance** to use the models
4. **The API timeout is set to 10 minutes (600000ms)** to handle longer operations
5. **The router runs on port 3456** - ensure this port is not blocked by firewall

### Qwen Model Options

You can use different Qwen models based on your needs:

- `qwen3-coder-plus`: Good balance of capability and speed - suitable for most tasks
- `qwen3-coder-max`: Most capable for complex tasks - better for reasoning and complex code
- `qwen3-coder-turbo`: Fastest for simple tasks - good for quick responses

#### Changing Models:
To use a different model, change the model name in the router configuration:
- From `"default": "qwen,qwen3-coder-plus"`
- To `"default": "qwen,qwen3-coder-max"` (for the more capable model)

### Additional Commands and Tips

#### Useful Claude Code Commands:
- `> help` - Show available commands
- `> /file <path>` - Include a file in the conversation
- `> /clear` - Clear the current conversation
- `> /exit` - Exit Claude Code

#### Router Management Commands:
- `ccr start` - Start the router
- `ccr stop` - Stop the router
- `ccr restart` - Restart the router
- `ccr status` - Check router status
- `ccr version` - Check router version

#### Configuration Updates:
If you need to update your configuration later:
1. Stop the router: `ccr stop`
2. Edit the config file: `notepad "$env:USERPROFILE\.claude-code-router\config.json"`
3. Restart the router: `ccr restart`

### Support and Resources

- Qwen Documentation: [https://qwen.ai](https://qwen.ai)
- Claude Code Documentation: [https://docs.anthropic.com](https://docs.anthropic.com)
- Claude Code Router: Check GitHub repository for updates

---

## Quick Verification Checklist

- [ ] Node.js version is 18 or higher (`node --version`)
- [ ] Qwen CLI is installed (`qwen --version` works)
- [ ] Claude Code is installed (`claude --version` works)
- [ ] Claude Code Router is installed (`ccr version` works)
- [ ] Qwen account is authenticated (`qwen` command works)
- [ ] Router configuration file exists at `~/.claude-code-router/config.json`
- [ ] Router starts successfully (`ccr start` shows "Service started successfully")
- [ ] Claude Code connects through router (`ccr code` opens Claude interface)
- [ ] Test command works (`> hi` returns a response from Qwen)