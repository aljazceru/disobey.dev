---
title: "262K context on 16GB VRAM because why not"
date: 2026-06-03
slug: "gemma-4-26b-262k-context-16gb-vram"
tags: ["AI", "llm", "self-hosted","gemma4"]
draft: false
---

I've been running local LLMs for a while now and the eternal struggle is always the same: you want more context, more model, more speed — and you have none of the VRAM to support any of it. So when Gemma 4 dropped with 262K context window I obviously had to try fitting the whole thing on my RTX A5000. 16GB. Turns out you can. And its actually usable.

~658 tok/s prompt eval. ~35 tok/s decode. Full 262K context window. f16 KV cache, no compression tricks.

### The model

The dense Gemma 4 31B doesn't fit in 16GB, simple as that. So the MoE variant — `gemma-4-26B-A4B-it-Q4_K_M.gguf` — is doing the heavy lifting here. Its a 26B parameter model but only activates 4B per token which is what makes this whole thing possible. I tried going with gemma4-31b, i like them dense as much as the next guy, but I was getting somewhere between 1–3 tok/s which is basically watching paint dry in slow motion. Switching to sweet saviour of us VRAM poor I got to 35–40 tok/s. Higher if you lower context. But where's the fun in that.

### The command

Its ugly. Every flag matters. Don't ask me which ones you can skip because I've tried skipping them and the answer is none.

```bash
llama-server \
  -m "$MODEL" \
  -c 262144 \
  -ngl auto -fit on -fitt 128 -fitc 262144 \
  -fa on \
  -ctk f16 -ctv f16 \
  -t 8 -tb 8 \
  -b 4096 -ub 1024
```

### What each flag actually does

Because I know you're going to copy paste this and then wonder why it works (same).

**`-c 262144`** — reserves the full 262K context window. This is the whole point of the exercise.

**`-ngl auto -fit on`** — this is the magic combo. It dynamically trades off GPU layers vs KV cache size so everything actually fits in VRAM. Without it you're either running out of memory or leaving performance on the table.

**`-fitt 128`** — keeps ~128 MiB of VRAM headroom. Because nothing ruins your day like an OOM error mid-inference.

**`-fitc 262144`** — tells the fitter not to silently shrink your context window behind your back. If you asked for 262K you get 262K.

**`-fa on`** — flash attention. Faster and better for long-context paths. Not optional at these context lengths.

**`-ctk f16 -ctv f16`** — f16 KV cache for better long-context recall. I tested q8 as well and it was sometimes faster but the quality degradation at longer contexts was noticeable. f16 is the sweet spot.

**`-t 8 -tb 8`** — 8 threads. This matched my physical cores best. Your mileage may vary but going higher didn't help on my setup.

**`-b 4096 -ub 1024`** — batch size 4096 for prompt eval, 1024 for micro-batch. Good prompt processing speed without blowing through memory. When I was testing with q8 KV cache I had to drop these to 2048/512.

### The takeaway

If you want long-context on 16GB VRAM: Gemma 4 26B-A4B, 262K context, f16 KV, llama.cpp CUDA, `-ngl auto -fit on`. Thats the best balance I've found — full context, clean recall, interactive speed.

No cloud API, no token counting anxiety, no context window roulette. Just a local model that can actually chew through a serious amount of text without crawling to a halt. The fact that it fits on a single GPU that you can buy used for reasonable money is kind of absurd when you think about it.

Now if you'll excuse me I have 262K tokens of context to fill with increasingly questionable prompts.
