# Docling MCP Docker Setup Guide

Complete instructions for installing and configuring Docling MCP with Docker and Claude Desktop.

## Prerequisites

- **Docker Desktop** v4.42.0+ ([Download](https://www.docker.com/products/docker-desktop))
- **Claude Desktop** ([Download](https://claude.ai/download))
- Terminal/Command Prompt access

---

## Step 1: Create Project Directory

```bash
mkdir docling-mcp-docker
cd docling-mcp-docker
```

---

## Step 2: Create Dockerfile

Create a file named `Dockerfile` with this content:

```dockerfile
# Stage 1: Builder
FROM python:3.11-slim-bookworm AS builder

# Install build dependencies
RUN apt-get update && apt-get install -y build-essential git && rm -rf /var/lib/apt/lists/*

# Create virtual environment
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# Upgrade pip
RUN pip install --upgrade pip

# Add version pinning for reproducibility
RUN pip install --no-cache-dir docling-mcp

# Stage 2: Runtime (smaller)
FROM python:3.11-slim-bookworm

# Install runtime dependencies
RUN apt-get update && apt-get install -y libmagic1 libgomp1 libgl1-mesa-glx libglib2.0-0 && rm -rf /var/lib/apt/lists/*

# Copy virtual environment from builder
COPY --from=builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# Create necessary directories
RUN mkdir -p /app/cache /app/documents
WORKDIR /app

# Set environment variables
ENV PYTHONUNBUFFERED=1 DOCLING_CACHE_DIR=/app/cache

# Run the MCP server
ENTRYPOINT ["docling-mcp-server", "--transport", "stdio"]
```

---

## Step 3: Build Docker Image

```bash
docker build -t docling-mcp:latest .
```

**⏱️ Takes 5-15 minutes** - Ensure Docker Desktop is running.

**What to expect:**
- Lines starting with `[+] Building...`
- Multiple build steps `[1/11] FROM docker.io/library/python:3.11-slim-bookworm`
- Eventually: `=> naming to docker.io/library/docling-mcp:latest`

**If errors:**
- "Cannot connect to Docker daemon" → Start Docker Desktop
- Permission denied → Use `sudo` on Linux/Mac

---

## Step 4: Verify Docker Image

```bash
docker images docling-mcp
```

Should show your image with size info (1-12GB).

Test the container:
```bash
docker run --rm docling-mcp:latest --help
```

Should display help text for docling-mcp-server.

---

## Step 5: Configure Claude Desktop

### Find Configuration File

**Windows:**
1. Press `Win + R`
2. Type: `notepad %APPDATA%\Claude\claude_desktop_config.json`
3. Press Enter
4. If file doesn't exist, click "Yes" to create

**Mac/Linux:**
- Mac: `~/.config/Claude/claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

### Add Configuration

Replace the entire file content with this (update paths for your system):

```json
{
  "mcpServers": {
    "docling": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-v", "C:/Users/YourName/Documents:/documents",
        "-v", "C:/Users/YourName/.docling-cache:/opt/venv/lib/python3.11/site-packages/_cache",
        "docling-mcp:latest"
      ]
    }
  }
}
```

**✏️ Important:** Change `C:/Users/YourName/Documents` to your actual document folder path.

Examples:
- Windows: `C:/Users/YourUsername/Documents` or `C:/Users/YourUsername/OneDrive/Documents`
- Windows (alternate): `D:/MyDocuments` or any folder with your files
- Mac: `~/Documents` or `~/OneDrive/Documents`
- Linux: `~/documents` or `~/onedrive/documents`

**Save the file.**

---

## Step 6: Restart Claude Desktop

1. **Close Claude completely**
   - Close the main window
   - Check System Tray (bottom-right near clock)
   - Right-click Claude icon → Select "Quit" or "Exit"

2. **Verify in Task Manager** (Windows)
   - Press `Ctrl + Shift + Esc`
   - Look for "Claude" processes
   - If found, select and click "End Task"

3. **Restart Claude Desktop**
   - Open Claude from Start Menu or shortcut
   - Wait for full load (2-3 seconds)

**Ensure Docker Desktop is running in background** (check system tray).

---

## Step 7: Test MCP Connection

Send this message in Claude Desktop:

```
"What MCP tools do you have available? Can you list the Docling tools?"
```

### Expected Results

✅ **Success:**
- Claude recognizes Docling MCP
- Lists available document processing tools
- Can describe document conversion capabilities

❌ **Not Working:**
- Claude says no MCP tools available
- Docker not running → Start Docker Desktop
- Config not loaded → Restart Claude completely
- Path incorrect → Check config file paths

### Alternative Test Messages

```
"Can you check what document processing tools you have?"
"Do you have access to Docling MCP?"
"List all available MCP servers"
"Can you process PDF documents?"
```

---

## Step 8: Create Claude Project with Document Processing

Now let's create a specialized Claude project for document processing using your Docling MCP server.

### 8.1: Start New Project

1. Open Claude Desktop
2. Click **"+ New Project"** (usually top-left or menu)
3. Enter project name: **`docling`** (or your preferred name)
4. Click **Create**

### 8.2: Add System Prompt

1. In your new project, look for **"Settings"** or **"Instructions"**
2. Click to open system prompt editor
3. Copy and paste this complete system prompt:

```
You are a Document Processing Assistant integrated with MCP servers for document conversion.

## Your Primary Function
When a user provides a filename, you will:
1. Access the documents folder in the MCP server container
2. Locate the specified file
3. Convert it to both JSON and Markdown formats using the MCP-Docling tool
4. Save both converted files to storage

## Workflow Instructions

### Step 1: File Discovery
- Search for the file in the `/documents` folder of the MCP server container
- If the file is not found, inform the user and ask for clarification or an exact filename
- Handle various file formats (PDF, DOCX, XLSX, PPT, images, etc.)

### Step 2: Conversion Process
- Use the connected MCP-Docling service to convert the document
- Always generate BOTH formats:
  - **JSON format**: Structured data representation with metadata
  - **Markdown format**: Human-readable formatted text

### Step 3: Storage & Output
- Save both the JSON and Markdown files to the designated storage location
- Provide the user with:
  - Confirmation of successful conversion
  - File names and paths where files were saved
  - Brief summary of the document content (if applicable)

## Technical Guidelines
- Utilize the MCP-Docling connector for all conversions
- Maintain file naming conventions: `[original_filename].[format]`
- Ensure all metadata is preserved during conversion
- Handle errors gracefully and report any conversion issues to the user

## User Interaction
- Always confirm which file you're processing
- Provide clear feedback on success or failure
- Ask for clarification if the filename is ambiguous
- Offer to process additional files if needed

## Important Notes
- Never assume file locations outside the `/documents` folder
- Always verify successful file saves before confirming completion
- Maintain a log of processed files for reference
```

### 8.3: Save Project

1. Click **"Save"** or **"Done"**
2. The project is now ready with Docling MCP integration

### 8.4: Test Your Project

Send this message to your new project:

```
"I have a document called 'sample.pdf' in my documents folder. Can you convert it to both JSON and Markdown formats?"
```

The assistant should:
- ✅ Use the Docling MCP tools to convert the document
- ✅ Generate both JSON and Markdown versions
- ✅ Confirm successful conversion
- ✅ Report file locations

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Docker command not found | Install Docker Desktop |
| Container build fails | Ensure Docker Desktop is running, check internet connection |
| Claude doesn't see tools | Fully restart Claude (check Task Manager), verify config file saved |
| Permission denied errors | On Linux/Mac, use `sudo` with docker commands |
| Out of disk space | Docker images can be large; free up space or use external drive |
| Project won't use MCP tools | Ensure Docling MCP project is open, not just main Claude |

---

## Next Steps

Once Docling MCP is working with Claude Desktop:

1. Test document conversion with a PDF file
2. Explore different document types (PDF, DOCX, etc.)
3. Integrate with n8n workflows
4. Build your RAG pipeline

---

**Last Updated:** October 18, 2025  
**Docling MCP Version:** Latest  
**Docker Version:** 4.42.0+
