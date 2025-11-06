---
title: "how to post videos everyone can watch"
date: 2025-05-22
slug: "how-to-post-videos-everyone-can-watch"
draft: false
---

The author addresses a persistent problem in 2025: video compatibility across devices. They note that while technology has advanced significantly, sharing videos that work universally remains challenging, particularly when iPhone users cannot open certain formats.

## Solution

The article recommends using FFmpeg with a bash function to convert videos to a universally compatible format. The proposed bash function is:

```bash
cvideo() { ffmpeg -i "$1" -c:v libx264 -crf 20 -preset slow -vf format=yuv420p -c:a aac -movflags +faststart "${1%_*}"_twitter.mp4; }
```

**Usage example:** `cvideo iphone_cant_watch_me.webm`

## Key Points

- This approach converts videos to MP4 format compatible across platforms
- The resulting files are suitable for social media uploads (Twitter, etc.)
- Users should add this function to their `.bashrc` or similar shell configuration file
- The solution leverages FFmpeg, established open-source software for multimedia handling

The author humorously contrasts modern codec issues with historical workarounds, suggesting that universal video compatibility "is something we need to wait for AGI to be achievable."
