# Abstract Syntax Tree MCP Client

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](../LICENSE)

Standalone reference web chatbot client for the AST MCP Server, featuring real-time AI assistant chat with Model Context Protocol (MCP) tool calling and interactive UI rendering.

## Table of Contents

- [Background](#background)
- [Install](#install)
- [Usage](#usage)
- [API](#api)
- [Contributing](#contributing)
- [License](#license)

## Background

`mcp-client` acts as a standalone reference client demonstrating how web applications can interface with `mcp-server` over Streamable HTTP transport.

_Note: The primary user-facing chatbot interface is integrated directly into `renderer` under the `/chat` route._

## Install

```console
cd mcp-client
npm install
npm run build
```

## Usage

### Development Mode

```console
npm run dev
```

### Production Mode

```console
npm run build
npm run start
```

Default URL: `http://localhost:3200`.

## API

### HTTP Endpoints

- **`GET /api/health`**: Returns system status, active LLM provider/model, MCP connection status, and tool count.
- **`GET /api/mcp/tools`**: Lists all tools discovered from the connected MCP server.
- **`POST /api/chat`**: Accepts `{ message, history }` payload and generates LLM responses, automatically calling MCP tools as needed.

### Environment Variables

| Variable                                              | Default                                               | Description                                                                          |
| ----------------------------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `PORT`                                                | `3200`                                                | HTTP port for the MCP web client server.                                             |
| `MCP_SERVER_URL`                                      | `http://localhost:4001/mcp`                           | Streamable HTTP endpoint of `mcp-server`.                                            |
| `CHAT_BOT_PROVIDER`                                   | `gemini`                                              | Active LLM provider (`gemini`, `anthropic`, `openai`, `ollama`).                     |
| `GEMINI_API_KEY` / `GEMINI_MODEL`                     | _(none)_ / `models/gemini-3.6-flash`                  | API key and model name for Google Gemini.                                            |
| `ANTHROPIC_API_KEY` / `ANTHROPIC_MODEL`               | _(none)_ / `claude-3-5-sonnet-20241022`               | API key and model name for Anthropic Claude.                                         |
| `OPENAI_API_KEY` / `OPENAI_MODEL` / `OPENAI_BASE_URL` | _(none)_ / `gpt-4o` / `https://api.openai.com/v1`     | API key, model and endpoint for OpenAI or any OpenAI-compatible service (see below). |
| `OLLAMA_BASE_URL` / `OLLAMA_MODEL`                    | `http://host.docker.internal:11434/v1` / `qwen2.5:3b` | Base URL and model name for Ollama.                                                  |
| `BOOTSTRAP_THEME`                                     | `"default"`                                           | Bootswatch theme name (`journal`, `darkly`, `flatly`, `cyborg`, etc.).               |
| `SHOW_TOOL_INVOCATIONS`                               | `false`                                               | Displays MCP tool call details and payload data in the UI response when true.        |

##### Other OpenAI-compatible providers

Any service exposing an OpenAI-style `/chat/completions` endpoint works through the `openai` provider: set `OPENAI_BASE_URL`, `OPENAI_API_KEY` and `OPENAI_MODEL`. The model must support tool (function) calling, since `/chat` relies on it.

| Service                                | `OPENAI_BASE_URL`                        | Example `OPENAI_MODEL`                   | Get a key                                                |
| -------------------------------------- | ---------------------------------------- | ---------------------------------------- | -------------------------------------------------------- |
| [Groq](https://console.groq.com)       | `https://api.groq.com/openai/v1`         | `llama-3.3-70b-versatile`                | <https://console.groq.com/keys>                          |
| [OpenRouter](https://openrouter.ai)    | `https://openrouter.ai/api/v1`           | `meta-llama/llama-3.3-70b-instruct:free` | <https://openrouter.ai/keys>                             |
| [NVIDIA NIM](https://build.nvidia.com) | `https://integrate.api.nvidia.com/v1`    | `meta/llama-3.3-70b-instruct`            | <https://build.nvidia.com> (open a model, "Get API Key") |
| [Cohere](https://cohere.com)           | `https://api.cohere.ai/compatibility/v1` | `command-r-plus-08-2024`                 | <https://dashboard.cohere.com/api-keys>                  |

Example `.env` for Groq:

```console
CHAT_BOT_PROVIDER=openai
OPENAI_BASE_URL=https://api.groq.com/openai/v1
OPENAI_API_KEY=gsk_...
OPENAI_MODEL=llama-3.3-70b-versatile
```

Model IDs and free-tier limits change often; check each provider's model list.

## Contributing

PRs accepted. Refer to the root repository contributing guidelines.

## License

[Apache-2.0](../LICENSE) © Jason Fox
