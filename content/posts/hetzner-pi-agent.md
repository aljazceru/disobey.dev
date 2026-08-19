---
title: "Using hetzner's free inference in your coding agent"
date: 2026-08-19
slug: "hetzner-free-inference-coding-agent-config"
tags: ["AI", "llm", "self-hosted","hetzner"]
draft: false
---
Hetzner is [offering some free inference](https://experiments.hetzner.com/docs/inference) and since free tokens are always welcome it would be a shame not to give the models for a spin. Specially since they now offer Qwen3.8-27B for free, which is perfect for those who don't have enough local compute to run it themselves (or those who need more tokens - and who doesnt).


Below are examples for two agents I regularly

## Pi configuration 
cat ~/.pi/agent/models.json
```bash
{
  "providers": {
    "hetzner": {
      "baseUrl": "https://inference.hetzner.com/api/v1",
      "api": "openai-completions",
      "apiKey": "$HETZNER_API_KEY",
      "compat": {
        "supportsDeveloperRole": false,
        "supportsReasoningEffort": false,
        "supportsStore": false,
        "maxTokensField": "max_tokens"
      },
      "models": [
        {
          "id": "Qwen/Qwen3.6-35B-A3B-FP8",
          "name": "Qwen3.6 35B A3B (Hetzner)",
          "reasoning": true,
          "input": ["text"],
          "contextWindow": 262144,
          "maxTokens": 32768,
          "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 }
        },
        {
          "id": "Qwen3.8-27B",
          "name": "Qwen3.8 27B (Hetzner)",
          "reasoning": true,
          "input": ["text"],
          "contextWindow": 262144,
          "maxTokens": 32768,
          "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 }
        }
      ]
    }
}
```

## Opencode configuration
cat .config/opencode/opencode.json
```bash
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "hetzner": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Hetzner",
      "options": {
        "baseURL": "https://inference.hetzner.com/api/v1",
        "apiKey": "{env:HETZNER_API_KEY}"
      },
      "models": {
        "Qwen3.8-27B": {
          "name": "Qwen3.8-27B"
        },
        "Qwen/Qwen3.6-35B-A3B-FP8": {
          "name": "Qwen3.6 35B A3B"
        }
      }
    }
  }
}
``` 
