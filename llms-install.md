# Installing the PeptideCompare.co MCP server

This is a hosted, remote MCP server. There is nothing to download, build or run, and no API key.

- Transport: Streamable HTTP
- URL: https://peptidecompare.co/mcp
- Authentication: none

Add it to the client's MCP settings as a remote server, for example:

```json
{ "mcpServers": { "peptidecompare": { "url": "https://peptidecompare.co/mcp" } } }
```

To check it, call `list_compounds`, which takes no arguments and returns every peptide, NAD+ and peptide stack the index prices, with its lowest and typical cost per 30 days.
