---
title: "Mango - Confidential AI app that puts you in control"
date: 2026-04-15
slug: "mango-confidential-ai"
tags: ["ai", "security", "privacy"]
draft: false
---
TLDR: No provider lock-in. No subscriptions. Own your data data. Bring your own key. Protect your chats. And delete them if forced to unlock the app. No tracking, no analytics. The app that serves you. 

Privacy is dead. Long live privacy. Wrong. There are still a few of us out there that believe in it. A few that are trying to build software for users that value it. Software that works for the user, not against him. And that is how Mango was born.
{{< figure
  src="mango-hero.webp"
  alt="Mango app promotional image"
  align="center"
  width="520"
>}}


The premise of Mango is simple: yet another chatgpt-like app (for now). But with a twist. It doesn't sell you a subscription. It works with only two providers for now: [Tinfoil](https://tinfoil.sh/) and [PPQ](https://ppq.ai/). More will be added. But not that many because there are very few out there who take confidential inference seriously. 

Mango works similarly than other AI apps you might have used before, but it has a few tricks up his sleeve. First and biggest one - confidential inference. Both providers currently available enable full remote attestation of their endpoints which Mango fully verifies itself. That means that the provider is providing cryptographic proofs of which software is running on their servers and in which environments it is running. That enables us to verify that their claims of confidential inference are actually true. What that means is that all the data you send to your AI agent remains confidential. The provider is not logging it, it is not gonna be used for training or to spy on you. It stays between you and your device.

But thats not all. It also offers on device embeddings - this means you can add documents to Mango and it will act as a mini RAG so you can attach those documents to your conversations and have the AI agent use them to answer your questions. It also supports tool use, currently limited to brave search. 

It also has a few security features that are not so standard. It automatically locks the app after a period of inactivity so you need to type in the pin to unlock it. It also supports biometrics for the phones that have them. It also has a duress pin - in case somebody is trying to force you to unlock the app, you can use the duress pin to unlock it. The app will delete all the conversations and documents and insert some random conversation in there to make it look like nothing happened.


{{< figure
  src="mango-lock.webp"
  alt="Mango app lock screen"
  align="center"
  width="300"
>}}



Currently its primarily available on android, desktop build works but needs a bit of love (only tested on linux). iOS is in the works but the lack of side loading makes it a bit more complicated since I'd need to beg the all mighty apple to let me open my kimono and publish it to the app store. Why would anyone use an ecosystem that doesn't let you actually do whatever you want with the hardware is another story.

I'll be adding more providers from [confidentialinference.net](https://confidentialinference.net/) which is another project of mine that aims to make confidential inference more accessible. The reality is that not all providers do a great job of actually exposing everything that's needed to verify their claims of confidential inference or they implement it in a way that requires you to run another side car process which doesn't really fit well with a mobile app.

Source code and apks are available on [github](https://github.com/aljazceru/mango) and the app is available on [zapstore](https://zapstore.dev/apps/dev.disobey.mango).
