---
title: "The road to tiles"
date: 2025-12-04
slug: "the-road-to-tiles"
tags: ["linux", "rant"]
draft: false
---
I've been using linux for many years now and every few years I get the urge to switch from GNOME to a tiling window manager. But every time I was tempted to make the switch, I ran into the same problem: the learning curve was just too steep. I don't want to spend hours configuring my system just to get it to a usable state. I treat my computer as  a tool, not a hobby project. Its one of the reasons I've been using ubuntu lts versions as my primary distro for so long. It mostly just works out of the box, it gives me the least headaches with my multi monitor setup and it has enough market share that most software vendors provide deb packages for it. I've used many other distros over the years, I used to be a huge Centos fan (until version 8). I've dabbled with Fedora years ago but I had issues with multi monitor setups back then. But lately there's been another itch. Possibly created by all the [Omarchy](https://omarchy.org) publicity by DHH discovering that linux is actually great. And since I've just gotten a new 4tb drive it kinda felt like maybe its the right time to try something new. And thats where the adventure starts...

#### Problem #1: Ubuntu and hyprland were not friends
Since hyprland is all the rage nowadays I figured I'd give it a try. But installing hyprland on ubuntu appears to not be a straight forward apt install due to all the outdated libs in ubuntu. Sure theres workarounds but as I said before, this is not a hobby project, its a tool that needs to work. And nobody is spending 2days polishing their wrench. So I decided to give Fedora another chance.

#### Probem #2: Fedora 43 apparently breaks hyprland
Its simple right? I install a fresh fedora on a spare laptop to test it out and see how I vibe with hyprland. Looks simple - just enable [copr repo](https://copr.fedorainfracloud.org/coprs/solopasha/hyprland/) and install hyperland. Not so fast sailor. Apparently fedora 43 broke hyprland due to some library changes. Off to a great start. Ok, theres other tiling window managers out there and since I have a test fedora setup now lets try something else since this is not inspiring confidence.

#### Problem #3: I remember why I never migrated to tiling WMs before
I tried several tiling window managers: Sway, i3 and awesomewm. At least they mostly worked on the test machine. But then I realized that defaults are so bare that we're back to spending tons of time to configure everything from scratch. I like usable defaults. Since I worked as sysadmin for many years its deeply ingraned in me that using default tools is the norm. (if I ever see custom vim plugins on a server you'll lose all respect from me). 

#### Problem #4: Why didn't I do this sooner?
After 4th test (this was a speed run through things, I spent like 10minutes evaluating each WM) I dawns on me that I didn't even do the most basic of things: googled for tiling managers for gnome. Time to feel retarded - turns out gnome has a perfect extension that solves all my problems - [Tiling Shell](https://extensions.gnome.org/extension/7065/tiling-shell/). No need to spend time on anything turns out. Fedora works smooth tho. Will likely end up on my main machine once I decide to actually unpack the nvme drive on my desk. 

I guess the moral of the story is that I'm getting old and lazy?
