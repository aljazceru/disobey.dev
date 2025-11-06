---
title: "how to post videos everyone can watch"
date: 2025-05-22
slug: "how-to-post-videos-everyone-can-watch"
draft: false
---

Its 2025 but sending a video from one person to another and assuming they will be able to open it on their device is still something we need to wait for AGI to be achievable.

Back in the day everyone had installed K-Lite codec pack to watch pirated movies. Now I'm trying to send product demos around and iphone users can't watch them. But luckily we have the almighty ffmpeg that always comes to the rescue. And this little known thing called bash functions that noone uses anymore.

In your .bashrc (or something else that will get loaded at login) add this little nugget:

```bash
cvideo() { ffmpeg -i "$1" -c:v libx264 -crf 20 -preset slow -vf format=yuv420p -c:a aac -movflags +faststart "${1%_*}"_twitter.mp4; }
```
and then you can easily convert any video to mp4 that will work anywhere (also great for uploads to twitter etc).

Example usage:
`cvideo iphone_cant_watch_me.webm 