# Inkscape MCP Server

This workspace now contains a self-contained MCP server for running an agentic design workflow through Inkscape 1.4.4.

The server speaks MCP over stdio, uses only the Python standard library, and wraps the local Inkscape CLI for SVG creation, inspection, validation, actions, and export.

Inkscape runtime profile/cache files are redirected into `.mcp-runtime/` by default so MCP tool calls do not need to write into the user profile. Set `INKSCAPE_MCP_ISOLATE_PROFILE=0` if you want Inkscape to use the normal profile directories.

Verified local binary:

```powershell
C:\Program Files\Inkscape\bin\inkscape.exe
Inkscape 1.4.4 (dcaf3e7, 2026-05-05)
```

## Quick Start

Run the smoke test:

```powershell
python .\scripts\smoke_test.py
```

Run the MCP server:

```powershell
python -m inkscape_mcp_server --workspace "C:\Users\Sandra\OneDrive\Desktop\git-repos\inkscape-mcp-server" --inkscape "C:\Program Files\Inkscape\bin\inkscape.exe"
```

Example MCP client config:

```json
{
  "mcpServers": {
    "inkscape": {
      "command": "python",
      "args": [
        "-m",
        "inkscape_mcp_server",
        "--workspace",
        "C:\\Users\\Sandra\\OneDrive\\Desktop\\git-repos\\inkscape-mcp-server",
        "--inkscape",
        "C:\\Program Files\\Inkscape\\bin\\inkscape.exe"
      ],
      "env": {
        "INKSCAPE_MCP_EXPECTED_VERSION": "1.4.4",
        "INKSCAPE_MCP_ISOLATE_PROFILE": "1"
      }
    }
  }
}
```

## Agent Workflow

The agent files in `agents/` define a practical design process:

1. `design-strategist.md` turns a request into a clear design brief.
2. `art-director.md` defines composition, palette, typography, and review criteria.
3. `vector-technician.md` produces editable SVG using the MCP tools and Inkscape.
4. `production-qa.md` validates, exports, and reports production issues.
5. `process.md` ties those roles together as the default end-to-end workflow.

The same workflow is also exposed through MCP prompts:

- `design-process`
- `art-director-review`
- `vector-production`
- `production-qa`

## MCP Tools

- `inkscape_version`: verify the Inkscape executable and version.
- `create_design_brief`: write a structured JSON brief into `briefs/`.
- `create_svg_document`: create an editable layered SVG starter document.
- `inspect_svg`: inspect SVG dimensions, layers, text, and element counts.
- `validate_design`: XML-parse and optionally render-check an SVG through Inkscape.
- `export_design`: export SVG through Inkscape to PNG, PDF, EPS, PS, or SVG.
- `run_inkscape_actions`: run explicit Inkscape action strings against a workspace SVG.

All paths are constrained to this workspace.
