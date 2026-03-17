---
title: "The end of 'trust me bro' - confidential computing for everyone"
date: 2026-02-06    
slug: "the-end-of-trust-me-bro-confidential-computing-for-everyone"
tags: ["confidential-computing","infrastructure", "privacy"]
draft: false
---

Since the dawn of computing we've been handing over our data to machines we don't control. First it was the mainframe in the basement, then the server in the data center, now the container in someone else's cloud. The bargain has always been the same: convenience in exchange for trust. You trust that the admin won't peek at your data. You trust that the code running is what they claim. You trust that nobody compromised the infrastructure while you weren't looking.

This trust is unverifiable. You simply hope.
![](https://blossom.primal.net/9db46516d241c3f75430d73dd0f34f27b6d45a8556e1b89d89a15fab5eaad027.jpg)


For most applications this is fine. Nobody cares if their todo list app operator is honest. But some data is different. Bitcoin private keys, medical records, your naked selfies (who am I kidding). For these, \"trust me bro\" has always been a deeply unsatisfying answer.

But it doesn’t need to be this way.

## Confidential computing history


Confidential computing is not a new thing. Intel SGX’s has existed long enough to be deprecated. It stands for Software Guard Extensions, and while it was far from great (unless you measure greatness in the amount of vulnerabilities) it kickstarted the confidential computing movement. It was also used for DRM so let's just say that it was not winning hearts and minds. Its design was also severely limited so it was more of a specific use case tool than a general solution. 

Lucky for us things have evolved a bit over the years. We now have AMD SEV-SNP and Intel TDX, both of which enable a new era of confidential computing - guest vm isolation. 

The core idea is simple. The CPU creates an encrypted virtual machine that nothing else can read. Not the operating system. Not the hypervisor. Not the cloud provider's staff. Not other processes on the same machine. The encryption keys exist only inside the CPU silicon and never leave it. That means data encryption in use, which closes the trifecta of data encryption.

![](https://blossom.primal.net/33b339932114f5070eaaa4b6e5b8b876e03e7e0f938ad4558ee262acc172c276.png)
## The second part that makes the magic

Running things confidentially is good, but if you run malicious code (looking at you DRM) instead of what you wanted the code itself can still leak your secrets. You need a second piece to seal the deal. You need to be able to ensure that a specific version of your code runs in the TEE (short for Trusted Execution Environment).

This is where remote attestation comes in.

A confidential vm is started inside a TEE. The platform computes measurements of the initial state it cares about (initial memory, config, data etc) and creates an attestation report containing those measurements and other metadata. The report is signed with a hardware-rooted attestation key, which only exists within the TEE. This is also where most of the trust assumptions in this setup lies - that hardware vendor didn’t leak or sell the root keys validating the certificate chain. \n\nWhat this process enables us is to ensure that we can be certain that a specific version of the software (or a specific docker image) is running there. Which also means that for verification to be possible the software needs to be open source (or at least you need to have access to it).\n\n## Enough with the theory \n\n##\n\nI've been thinking about this stuff for a while, initially through the lens of confidential data vending machines for nostr during SEC-01. I have since then spent a lot of time digging into confidential computing and especially confidential inference that was enabled with the Blackwell generation of Nvidia chips. But as they say: show, don’t tell.

Last week I've announced the[ first cashu mint in TEE](https://github.com/aljazceru/nutshell-azure-tee) on [nostr](https://njump.me/nevent1qqsp9rnxv5d6hq8465attxc6luywdxdzekleztkr7e782td4xwuy2fg285jcl)

Cashu mints are custodial by design. You deposit bitcoin, you get ecash tokens. The mint holds your funds. Well technically whoever runs the bitcoin backend of the mint holds the funds. But a lot of things could still go wrong. Mints can get compromised. Operators can install tracking software. Or maybe they are just running an old vulnerable version of the mint software. Those are just a few examples of why ensuring that the mint you are connecting to is running a specific and even better, vetted version of the mint software is very beneficial. 

With a TEE mint, the game changes. Users can request an attestation report, verify the signature and confirm the mint is running exactly the code it claims. And that it's running in a confidential container, ensuring that things are not leaking anything. Not because the operator said so, but because cryptography proved it.\n\nNow replace cashu mint with anything else you care about. You could run a Lightning node where even your hosting provider can't access the keys. Or a multisig coordinator. Custodians that prove their security posture cryptographically. Not \"we passed an audit\" but \"here's proof our systems are configured correctly right now.\". Run AI inference on private data without exposing inputs to the model operator. Prove which model version generated an output. \n\nVPNs that cryptographically prove they don't log. Messaging relays with verifiable no-retention. \n\nNo more policy documents and pinky promises.\n\n## The (bad) trust chain\n\nYes, there is some trust involved. It's not a panacea, it's just better than what we have now.\n\nWhen you verify an attestation report, you're trusting:
\- Manufacturers root of trust, revocation and TCB reporting
- The expected measurements you're comparing against are the real ones
- Attestation service, if you’re using it instead of doing the full verification yourself 

(if you want to skip the really geeky part scroll down to So what?)

### Confidential containers on Azure
So much for the theory. How does this work in practice? 
I built Nutshell TEE on Azure Confidential Containers, which run on AMD SEV-SNP hardware. Azure handles a lot of the complexity, but understanding the pieces helps. \n\nA confidential container group on Azure consists of multiple containers running together inside a single TEE boundary. The entire group shares the encrypted memory space. In my case, the architecture has four containers:\n\nThe SKR sidecar handles attestation. It talks to the AMD hardware to get the raw attestation report, then sends it to Microsoft's Azure Attestation service (MAA) which validates the report and returns a signed token. This token is what external clients verify.\n\nThe encfs sidecar mounts encrypted storage. The database lives on a LUKS-encrypted filesystem, and the decryption key can only be released inside the TEE.\n\nCaddy handles HTTPS termination. And the Nutshell mint runs the actual Cashu protocol. \n\nSo the deployment is pretty heavy in terms of infrastructure. It is much easier to deploy stateless services (you get rid of the encfs sidecar and the complexity around that).\n\n### Verifiable software identity\n\nThe clever part of Azure's implementation is how they handle software identity.\n\nBefore deployment, you define a policy that specifies exactly which container images are allowed to run, down to the layer digests. This policy gets hashed, and that hash (Azure calls it \"hostdata\") becomes part of the attestation token.\n\nWhen a client wants to verify the mint:\n\n1\\. They fetch the attestation token from the mint\n\n2\\. They verify the token's signature against Microsoft's public keys\n\n3\\. They extract the hostdata hash from the token\n\n4\\. They compute the expected hash from the published policy\n\n5\\. If the hashes match, they know the exact code running\n\nThe policy is public, in the git repo. Anyone can compute the expected hash and compare it against what the attestation token reports. If they match, you have cryptographic proof that the mint is running exactly the code you see in the repository.\n\n### Key release\n\nThe encryption key for the database adds another layer.\n\nIt's an RSA-HSM key stored in Azure Key Vault with a release policy tied to the hostdata hash. If anyone tries to access that key from outside the enclave, or from an enclave running different code, Key Vault refuses. The key literally cannot be released unless the attestation claims match the policy.\n\nThis means even if someone steals the encrypted database blob, they can't decrypt it without running the exact same code in a genuine TEE. The cryptographic binding between \"what code is running\" and \"what secrets are accessible\" is enforced by hardware and the key vault, not by hopes and prayers.\n\n## So what?\n\nBitcoin gave us verifiable money. Confidential computing gives us verifiable execution. And a confidential one at that. With the advancement of confidential containers and broader availability of hardware (for example you can rent a server that supports AMD SEV-SNP for 200$/month) things have become much easier. With AWS Nitro or SGX based solutions you generally need to architect your application for the specific platform which causes a lot of overhead (and vendor lock in). With confidential containers you can theoretically just throw any container in it and it will work. In practice you will need to at least figure out how to handle storage persistence and make sure that your builds are reproducible. But that's nothing compared to needing to fit your entire app into 256MB(sgx) or make it work over unix socket communication (aws nitro). 

In the example above I used azure just because the infrastructure is already deployed and set up. But you could deploy a similar type of infrastructure anywhere and self host the complete confidential containers stack. Ideally more infrastructure providers would agree on using exactly the same stack (big cloud providers inevitably tie it into their existing authentication which makes that part tied to the platform) so that users would have an easy way switching between providers if necessary. 

We should make this a default for any project or developer in the space, not a privilege used by well funded institutions like Signal. What we need is to make it very simple for every app developer and infrastructure operator to make use of these technologies, make it easy to devs to integrate attestation libraries in their wallets and clients and harden the infrastructure across the entire decentralized 
universe.

![Encrypt all the things! - Server Fault Blog](https://blog.serverfault.com/files/2016/02/encrypt-all-the-things1.png)


Reading materials:\\
- https://confidentialcontainers.org/
- https://ungovernable.tech/ConfidentialComputing.html
- https://github.com/aljazceru/nutshell-azure-tee\