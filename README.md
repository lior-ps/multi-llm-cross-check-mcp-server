[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/lior-ps-multi-llm-cross-check-mcp-server-badge.png)](https://mseep.ai/app/lior-ps-multi-llm-cross-check-mcp-server)

# Multi LLM Cross-Check MCP Server

A Model Control Protocol (MCP) server that allows cross-checking responses from multiple LLM providers simultaneously. This server integrates with Claude Desktop as an MCP server to provide a unified interface for querying different LLM APIs.

## Features

- Query multiple LLM providers in parallel
- Currently supports:
  - OpenAI (ChatGPT)
  - Anthropic (Claude)
  - Perplexity AI
  - Google (Gemini)
- Asynchronous parallel processing for faster responses
- Easy integration with Claude Desktop

## Prerequisites

- Python 3.8 or higher
- API keys for the LLM providers you want to use
- uv package manager (install with `pip install uv`)

## Installation

1. Clone this repository:

```bash
git clone https://github.com/lior-ps/multi-llm-cross-check-mcp-server.git
cd multi-llm-cross-check-mcp-server
```

2. Initialize uv environment and install requirements:

```bash
uv venv
uv pip install -r requirements.txt
```

3. Configure in Claude Desktop:
   Create a file named `claude_desktop_config.json` in your Claude Desktop configuration directory with the following content:

   ```json
   {
     "mcp_servers": [
       {
         "command": "uv",
         "args": [
           "--directory",
           "/multi-llm-cross-check-mcp-server",
           "run",
           "main.py"
         ],
         "env": {
           "OPENAI_API_KEY": "your_openai_key",  // Get from https://platform.openai.com/api-keys
           "ANTHROPIC_API_KEY": "your_anthropic_key",  // Get from https://console.anthropic.com/account/keys
           "PERPLEXITY_API_KEY": "your_perplexity_key",  // Get from https://www.perplexity.ai/settings/api
           "GEMINI_API_KEY": "your_gemini_key"  // Get from https://makersuite.google.com/app/apikey
         }
       }
     ]
   }
   ```

   Notes:

   1. You only need to add the API keys for the LLM providers you want to use. The server will skip any providers without configured API keys.
   2. You may need to put the full path to the uv executable in the command field. You can get this by running `which uv` on MacOS/Linux or `where uv` on Windows.

## Using the MCP Server

Once configured:

1. The server will automatically start when you open Claude Desktop
2. You can use the `cross_check` tool in your conversations by asking to "cross check with other LLMs"
3. Provide a prompt, and it will return responses from all configured LLM providers

## API Response Format

The server returns a dictionary with responses from each LLM provider:

```json
{
    "ChatGPT": { ... },
    "Claude": { ... },
    "Perplexity": { ... },
    "Gemini": { ... }
}
```

## Error Handling

- If an API key is not provided for a specific LLM, that provider will be skipped
- API errors are caught and returned in the response
- Each LLM's response is independent, so errors with one provider won't affect others

[![Verified on MseeP](https://mseep.ai/badge.svg)](https://mseep.ai/app/1f395e5c-f462-4250-8408-afd1ada852e5)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
