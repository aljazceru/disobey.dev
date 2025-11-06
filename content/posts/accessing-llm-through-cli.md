---
title: "quickly accessing llama3.2 from a terminal (or any other model)"
date: 2024-10-18
slug: "accessing-llm-through-cli"
tags: ["AI", "Infrastructure"]
draft: false
---

The author describes their workflow for integrating large language models into terminal-based work. Rather than switching to a browser or relying on a Telegram bot, they set up a more seamless solution using a remote GPU instance.

## Technical Setup

The solution involves three key components:

1. **Remote Infrastructure:** A cloud GPU instance running Ollama, accessible via SSH port forwarding (port 11434)
2. **Web Interface Option:** Open WebUI for browser-based access
3. **CLI Tool:** The `llm` package with Ollama plugin support

Installation steps are straightforward:
- Install the main CLI tool
- Add Ollama plugin support
- Set Llama3.2 as the default model

## Usage Examples

**Simple Query:**
The tool handles basic prompts directly from the command line with natural conversational responses.

**System Prompts:**
Users can pipe code or text through the tool with custom instructions, such as requesting code explanations.

**Chat Mode:**
Interactive sessions allow extended conversations, though the author finds this less practical than web UI alternatives.

## Limitations

The author notes the model includes safety guidelines that prevent certain types of content, and suggests exploring uncensored alternatives for less restricted interactions.
