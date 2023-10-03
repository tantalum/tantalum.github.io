---
title: "Software Supply Chain Attacls"
layout: post
date: 2023-10-03
tags: wifi, security
---

Some might say I am little late to the game writing about the 'colors' dependency sabotage [1] event now that it's  The fact of the matter is that this wasn't the first time we've seen supply chain attacks against NPM. Back in 2021 a couple of JS libraries were hijacked to include crypto miners [2]. Nor is hijacking dependencies limited to NPM packages, PyPI has had its own fun with supply chain attaches as well [3, 4].

In response a whole industry of DevSecOps vendors has emerged to scan all your dependencies and make sure they are safe. As wonderful as that is, the industry has remained reactive to these supply chain attacks. To take a lesson from the Anti Virus industry, automated scanning can only go so far detecting malicious code. I am not saying that dependency scanning is not a good thing, but it is not sufficient either. 

Another common tactic for mitigating supply chain attacks is code signing. This practice has been long-used in the Linux kernel, but that is also a much different use case than dependency management. More recently there has been an attempt at code signing PHP libraries [5]. However when the attacker is the "trusted" maintainer of the project, as it was with the 'colors' case, they can just sign the malicious code and push everything through. In other words, no amount of code signing by the maintainer would have prevented the 'colors' debacle.

One reason these attacks have become so popular is, like always, there is asymmetry between the cost to the attacker and the damage that is caused to the defender. As the industry continues to rely more and more on smaller and smaller Open Source libraries and projects, the effectiveness of these attacks will grow and the cost will continue to drop. The only way for the industry to effectively combat these kinds of supply chain attacks is to increase the cost of performing these attacks. For larger Open Source projects, this is mostly a solved problem. Thanks to OSS foundations, like the Apache Software Foundation and the FSF, Open Source project risk loosing their funding if malicious code is introduced into their projects. These foundations also provide solid governance and transition frameworks for open source project, creating a path for transitioning maintainers while maintaining trust in the project. However, for smaller Open Source libraries there is no funding or revenue source to be lost if malicious code is introduced into the project.

As the entropy in the Software Development industry grows, so will the industry's reliance on smaller projects and libraries. One of the cheapest options for increasing the cost of a supply chain attacks for these smaller libraries might be to find a way to fund them. This funding will both provide a cost to introducing malicious code, and an incentive to maintain the quality of the code. 

[1] https://blog.sonatype.com/npm-libraries-colors-and-faker-sabotaged-in-protest-by-their-maintainer-what-to-do-now
[2] https://thehackernews.com/2021/10/malicious-npm-packages-caught-running.html
[3] https://blog.sonatype.com/sonatype-catches-new-pypi-cryptomining-malware-via-automated-detection
[4] https://www.bitdefender.com/blog/hotforsecurity/supply-chain-attack-detected-in-pypi-library/
[5] https://paragonie.com/blog/2022/01/solving-open-source-supply-chain-security-for-php-ecosystem
