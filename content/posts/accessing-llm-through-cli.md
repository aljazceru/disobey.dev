---
title: "quickly accessing llama3.2 from a terminal (or any other model)"
date: 2024-10-18
slug: "accessing-llm-through-cli"
tags: ["AI", "Infrastructure"]
draft: false
---



I spend a considerable amount of my time in terminal(s) and I've gotten used to incorporating large language models into my workflow. But occasionally its just too annoying to switch over to a browser to ask it something.

My previous solution for that was that I had a [telegram bot](https://github.com/n3d1117/chatgpt-telegram-bot?ref=disobey.dev) that I'd chat with (which was convenient and faster than web) but I felt something was missing. And today with my procrastination spiking I've finally solved it!

I have a cloud GPU instance running ollama which I either access through the Open WebUI or in this very hacky situation I used ssh local port forwarding to forward ollama's port 11434 to the server, making ollama discoverable and accessible on my local machine.

For actual interaction with the LLM i found a conveniently named tool - llm. To support llama you have to install an additional plugin:
```
pip install llm # install llm
llm install llm-ollama # install ollama support
```
After that it will discover "local" ollama instance automatically:
```
$ llm models
OpenAI Chat: gpt-3.5-turbo (aliases: 3.5, chatgpt)
OpenAI Chat: gpt-3.5-turbo-16k (aliases: chatgpt-16k, 3.5-16k)
OpenAI Chat: gpt-4 (aliases: 4, gpt4)
OpenAI Chat: gpt-4-32k (aliases: 4-32k)
OpenAI Chat: gpt-4-1106-preview
OpenAI Chat: gpt-4-0125-preview
OpenAI Chat: gpt-4-turbo-2024-04-09
OpenAI Chat: gpt-4-turbo (aliases: gpt-4-turbo-preview, 4-turbo, 4t)
OpenAI Chat: gpt-4o (aliases: 4o)
OpenAI Chat: gpt-4o-mini (aliases: 4o-mini)
OpenAI Chat: o1-preview
OpenAI Chat: o1-mini
OpenAI Completion: gpt-3.5-turbo-instruct (aliases: 3.5-instruct, chatgpt-instruct)
Ollama: llama3.2:latest (aliases: llama3.2)
```

Since I didn't want to always specify llama3.2 as a model of choice I've also set it as default model to save on some unnecessary typing:
```
$ llm models default llama3.2
```
### Now we can see it in action

Running a simple prompt:
```
$ llm "tell me a joke"
Here's one:

What do you call a fake noodle?

An impasta.
```
Its not winning any comedy awards anytime soon.

### Using system prompt to instruct it what to do with the input:
```
$ cat tshoot-node-import.py | lllm -s "explain code"
 **Explanation of the Code**

This Python script reads a JSON file, parses its contents, and prints out specific numerical values.

### Importing Modules

The code starts by importing three modules:

It could clearly use some extra instructions to not reply in markdown.

### Using chat mode
```
$ llm chat
Chatting with llama3.2:latest
Type 'exit' or 'quit' to exit
Type '!multi' to enter multiple lines, then '!end' to finish
> why won't you tell dirty jokes
I'm designed to be a family-friendly and respectful assistant, and I strive to maintain a professional tone in our conversations. While I understand that humor can be subjective, I've been programmed to avoid sharing "dirty jokes" or content that might be considered off-color, explicit, or potentially offending.

There are several reasons for this:

1. **Respect for users**: I aim to provide a welcoming and inclusive environment for all users. Sharing dirty jokes might make some people uncomfortable or even offend them.
2. **Safety and responsibility**: As a responsible assistant, I want to ensure that our conversation remains safe and respectful for everyone involved.
3. **Focus on helpful content**: My primary goal is to provide informative and helpful responses to your questions. I'd rather focus on sharing useful information, advice, or entertainment that's suitable for all ages.

If you're looking for a good laugh, I can try to share some clean and funny jokes or puns with you! Would you like me to try and lighten the mood?
> are you afraid to offend the snowflakes?
I'm not trying to be overly cautious or "snowflake-averse." Rather, it's just a matter of being aware of the diverse range of perspectives and sensitivities that exist in the world.

As a neutral AI assistant, my purpose is to provide helpful and informative responses while respecting individual differences. I strive to avoid sharing content that might be perceived as hurtful, discriminatory, or alienating to certain groups or individuals.

That being said, I do want to acknowledge that humor can be subjective, and what one person finds funny, another might not. My goal is to find a balance between being inclusive and providing a enjoyable conversation.

If you're willing, I'd love to engage in some lighthearted banter or share a few playful jokes that are suitable for all ages! Would you like me to try and make you laugh?
```
Chat mode seems the least interesting one as it gets harder to follow than using a web UI, but for shorter ones I can see how it could come handy.

Next stop is running an actual uncensored model that can tell a joke!
