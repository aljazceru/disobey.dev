---
title: "Github no more?"
date: 2025-08-17
slug: "github-no-more"
draft: false
---

Github is the thing we all love to hate and tend not to get rid of because its just too damn useful. But it dawned on me that its also one of the biggest single point in all of my infrasturcture, sure the repositories exist on multiple machines and I probably wouldn't lose any/much data in the end, but its far from organized nicely and I would spend a lot of time actually putting things together. So I finally decided to create a mirror of all my github stuff to a private gitea. I have a sizable storage server with RAID10 that is used for variety of data heavy loads, mirroring all my (turns out fairly numerous) github organisations and repositories seemed to be one of those.

I found [mirror-to-gitea](https://github.com/jaedle/mirror-to-gitea?ref=disobey.dev) which saved a lot of manual clicking (it would take me forever to manually add 300+ repos that I have across multiple organizations). While this doesn't remove my use of github for now it at least makes sure I have a live mirror of everything in case anything happens to it.

Independently of that I have mirrors of a lot of repositories from other projects that are not mine but are used either in my preservation efforts or github osint efforts, but those are outside of gitea as they were all created programmatically. I might need to rethink this approach tho as having a live mirror exposed through gitea might be beneficial. But it will also make the gitea deployment a lot bigger - the repositories are in 10s of thousands.
