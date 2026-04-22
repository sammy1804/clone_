---
name: setup-stitch-mcp
description: Add the Stitch MCP server to this project's .mcp.json. Stitch is a Google Cloud MCP server. Pass your GCP project ID as the argument, or omit it to use the placeholder and fill it in later.
argument-hint: "[YOUR_GCP_PROJECT_ID]"
user-invocable: true
---

# Setup Stitch MCP

You are configuring the Stitch MCP server for this project.

**Argument:** `$ARGUMENTS` — the Google Cloud project ID to use. If blank, use the placeholder `YOUR_PROJECT_ID`.

## Steps

1. **Determine the project ID.**
   - If `$ARGUMENTS` is non-empty, use it as-is.
   - If empty, use `YOUR_PROJECT_ID` as the placeholder value and tell the user to replace it later.

2. **Check for an existing `.mcp.json`** at the project root.
   - If it exists, read it and merge the stitch server into the existing `mcpServers` object. Do not remove any existing servers.
   - If it does not exist, create it from scratch.

3. **Write `.mcp.json`** with the stitch server entry:

```json
{
  "mcpServers": {
    "stitch": {
      "command": "npx",
      "args": ["-y", "stitch-mcp"],
      "env": {
        "GOOGLE_CLOUD_PROJECT": "<project-id>"
      }
    }
  }
}
```

Replace `<project-id>` with the value from step 1.

4. **Validate** the written JSON is syntactically correct:
   ```
   jq empty .mcp.json
   ```
   Fix any syntax errors before continuing.

5. **Commit the change** with a clear message, e.g.:
   ```
   Add stitch MCP server configuration
   ```

6. **Tell the user:**
   - What was written and where.
   - If the placeholder was used, remind them to replace `YOUR_PROJECT_ID` in `.mcp.json` with their actual GCP project ID before using the server.
   - That they may need to restart Claude Code (or open `/mcp`) for the server to appear.
