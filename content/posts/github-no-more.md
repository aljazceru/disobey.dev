---
title: "Github no more?"
date: 2025-08-17
slug: "github-no-more"
draft: false
---

The author discusses concerns about GitHub representing a critical infrastructure vulnerability. While repositories exist across multiple machines, they lack proper organization, making recovery time-consuming.

To mitigate this risk, the author created a private Gitea mirror of all GitHub repositories. Using the tool "mirror-to-gitea," they successfully mirrored over 300 repositories across multiple organizations without manual configuration.

Beyond personal repositories, the author maintains mirrors of third-party projects for preservation and OSINT purposes. These are currently stored separately but could potentially be integrated into Gitea, though this would significantly increase deployment size—potentially to tens of thousands of repositories.

The author acknowledges this approach doesn't eliminate GitHub dependency but ensures a comprehensive backup exists. They describe the solution as providing security through redundancy while maintaining the practical utility GitHub currently offers.
