---
title: "Talking to machines or how i created whisper-talk"
date: 2026-01-17
slug: "talking-to-machines-or-how-i-created-whisper-talk"
tags: ["ai", "tools","whisper"]
draft: false
---
(in case you just want the github link without reading the halucinations of my neural network - [here you go](https://github.com/aljazceru/whisper-talk))


I hate voice memos. Mostly I hate to receive them. You can't quickly glance over to remember the content of the conversation, the key piece of information you need is hidden in a 3minute voice note. But I recently learned that talking to machines is quite practical. 

I even tweeted (its forever gonna be tweeting) about [talking to an LLM while pooping](https://x.com/aaaljaz/status/2012428125578182954). Not that I haven't used the feature before a few times but I never got the hang of it because I like to brain dump but then I want to read the reply (my brain works funny). 
I've even created a bot a while back that would transcribe signal voice memos that I braindumped to it, but it didn't stick, Until I've come across [hyprwhspr](github.com/goodroot/hyprwhspr) about a week or two ago. I immediately liked the idea, I've done several whisper projects in the past, [whisper-rs-cli](https://github.com/aljazceru/whisper-rs-cli) just this week so I dug into it. I wanted it as lean as possible so I first created a stripped down fork of it and fitted it to my fedora/ubuntu setup. But it was somewhat slowish in python and I've just came out of doing [whisper-rs-cli](https://github.com/aljazceru/whisper-rs-cli) so the decision was obvious. I need a rust version. Given that I had a working python version that I liked I thought its gonna be a slightly more straightforward path. It wasn't. Specially when I went down the path of "lets add real time typing into any app just for giggles" path. Since I've done some real(ish) time [transcription api experiments](https://github.com/aljazceru/transcription-api) before I thought that will also just be an extra half an hour. 

Narrator: it wasn't.

In the end I realized that I don't really WANT that functionality, I just thought it will look cooler in the README so saner thoughts prevailed, I pushed the code to a branch in case I want to procrastinate at some later time and viola, we're back at actually working version of the thing that I wanted. Which is [whisper-talk](https://github.com/aljazceru/whisper-talk).

Pure simplicity:
- press SUPER+ALT+d to start recording
- braindump everything you want
- press SUPER+ALT+d to stop recording and transcribe
- almost instantly the text is already in your clipboard (if you're using gpu) so you can paste it wherever you want

I've been using the python version of it for couple days and it became an almost natural part of my workflow already. I mostly use it for writing prompts, and given how much of them I write my fingers are very happy about it.