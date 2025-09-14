# OCI Pricing MCP Server (oci-pricing-mcp-server.py)

A Python-based MCP (Model Context Protocol) server for fetching Oracle Cloud Infrastructure (OCI) service pricing. It exposes tools that query Oracle's public Price List API (cetools) and return simplified JSON responses for integration with MCP-compatible clients.

## Overview

`oci-pricing-mcp-server.py` is a FastMCP server that lets you retrieve pricing information by SKU or by product name. It normalizes common aliases, performs fuzzy name matching, and enriches results with per-SKU pricing when available.

## Features

- **SKU Lookup**
  - Retrieve detailed pricing for a specific part number
- **Fuzzy Product Search**
  - Search by product name or alias with optional currency selection
  - Enrich results via per-SKU fetch to ensure pricing is populated
- **Structured Responses**
  - Returns JSON dictionaries ready for client-side rendering
- **Health Check**
  - Simple `ping` tool for connectivity testing

## Prerequisites

- Python 3.11+
- Internet access (uses Oracle's public Price List API)
- Dependencies listed in `pyproject.toml` (installed automatically during setup)

## Installation

1. Clone this repository.
2. Install the package so that dependencies from `pyproject.toml` are resolved:
   ```
   cd src/oci-pricing-mcp-server
   pip install -e .
   ```

## MCP Server Configuration

To use the server with an MCP-compatible client such as **Claude Desktop**, add an entry to `claude_desktop_config.json`:

```
{
  "mcpServers": {
    "ociPricing": {
      "command": "python",
      "args": ["/path/to/oci-pricing-mcp-server.py"]
    }
  }
}
```

Adjust the paths for your environment. On Windows this might look like `"C:\\Python\\python.exe"` with the script located at `"C:\\path\\to\\oci-pricing-mcp-server.py"`.

## Usage

The server uses stdio transport and can be started with:

```
python oci-pricing-mcp-server.py
```

## API Tools

1. `pricing_get_sku(part_number, currency="USD", max_pages=6)`: Fetch pricing for a given SKU.
2. `pricing_search_name(query, currency="USD", limit=12, max_pages=6, require_priced=False)`: Fuzzy search by product name; optionally require priced items only.
3. `ping()`: Health check returning `"ok"`.

## Notes

- The Price List API is a public subset; some queries may return empty `items`.
- Currency codes are ISO strings (e.g., `"USD"`, `"JPY"`).
