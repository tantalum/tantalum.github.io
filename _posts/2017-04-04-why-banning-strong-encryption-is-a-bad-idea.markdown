---
layout: post
title: "Why Banning End-to-End Strong Encryption is a Bad Idea(TM)"
---

## Introduction

> Those who would give up essential Liberty, to purchase a little temporary Safety, deserve neither Liberty nor Safety.

> <cite>Benjamin Franklin</cite>

There has been a recent push by [governments](http://www.telegraph.co.uk/news/uknews/terrorism-in-the-uk/11970391/Internet-firms-to-be-banned-from-offering-out-of-reach-communications-under-new-laws.html) and [government agencies](http://www.zdnet.com/article/fbi-chief-strong-encryption-lets-bad-guys-communicate-with-impunity/) to ban End-to-End Strong Encryption, all under the guise of stopping terrorists and pedophiles. My hope here is to provide simple arguments in layman's terms as to why banning strong end-to-end encryption will not improve the government's, nor it's agencies', ability to catch the "Bad Guys". In fact banning strong encryption will only daemonize legitimate users of strong encryption and undermine the rest of the population's security, while having almost no effect on those who wish to use it for nefarious means.

<!--more-->

**TL;DR:** Strong end-to-end encryption will remain accessible to those who really want it, whether it is legal or not. Thus removing strong end-to-end encryption from the hands of the public will not remove it from the hands of pedophiles and terrorists and will only make law abiding citizens' private data vulnerable.

## Encryption Only Works if it is in the Public Domain

In other words secret encryption algorithms aren't possible. For most people this will be hard to grasp because, from their perspective, encryption algorithms are already secret. Most people don't know that how [RSA](https://en.wikipedia.org/wiki/RSA_%28cryptosystem%29) works is public knowledge and can be easily looked up, and even if they did they would have a hard time understanding it. But for cryptographers it's always assumed that the encryption algorithm is known for two main reasons.

The first reason is somewhat technical, called [Kerckhoffs's principle](https://en.wikipedia.org/wiki/Kerckhoffs's_principle), *A cryptosystem should be secure even if everything about the system, except the key, is public knowledge*. I won't dwell on the principle too much simply because for most people the algorithm is already unknown, but it is assumed that how the algorithm works is available for those who want to know.

The second reason that an encryption algorithm can't be secret is because everyone that implements the algorithm needs to know it. This means that programmers, researchers, chip manufacturers and anyone else whose systems relies on encryption has to know the algorithm so they can implement it. It is impossible to implement something without knowing how it works, and with such a large body of, albeit technical, people knowing the algorithm it will be impossible to keep the algorithm secret for long.

## Back Doors and Golden Keys Don't Work

Back doors don't work for the simple reason that once it is discovered then anyone can use it. The software industry is full of examples of back doors that were exploited long before their existence became public. As [Bruce Schneier said](https://www.linuxvoice.com/a-quick-chat-with-bruce-schneier/):

> I can't design a filter that filters for morality. So if the US government can break in, anyone else can break in.

There are also calls for a ["Manhatten Project"](http://arstechnica.com/tech-policy/2015/12/hillary-clinton-wants-manhattan-like-project-to-break-encryption/) to create a secure back door in encryption systems, however the same logic stated above applies to all back doors. A back door is a still a back door no matter how sophisticated it is. To illustrate how even a "Mangatten Project" style effort can fail we only have to look as far as Digital Rights Management (DRM). Some of the brightest and best minds in technology have tried to come up with a DRM scheme that will allow legitimate users to play their media while blocking pirates from stealing that media. Every effort towards DRM has failed, usually in an unexpectedly short period of time. Although with DRM most end users will not know how to illegally copy their data, and most won't care to know, those who truly want to break DRM to copy files can easily do so with a little extra effort and almost no specialized knowledge once the initial scheme has been broken.

Another illustration of a "Manhatten Project" style backdoor effort is the infamous [Clipper Chip](https://en.wikipedia.org/wiki/Clipper_chip). Although with the Clipper Chip no method was discovered to illegitimately decode an encrypted message (keep in mind people didn't look that hard either), what was discovered was a method to encrypt messages using the chip that could *not* be decoded by a legitimate authority, i.e law enforcement.

## Strong Encryption Will Remain Accessible to Those Who Want to Use it

Strong encryption algorithms are publicly available today with no known weaknesses. Until all the currently available encryption algorithms are broken they will continue be an effective and secure way to encrypt data. Thus, even if a "Magic Backdoor Encryption" algorithm was discovered tomorrow pedo-terrorist will still have access to unbreakable strong encryption. If the government were to decide that it is illegal to use any encryption scheme other then "Maig Backdoor Encryption" then the only people effected will be legitimate corporations and law abiding citizens. The pedo-terrorists will continue to use the currently unbreakable encryption and the government won't gain any extra access into their communication.

The argument could be made that since the only people left using the now-illegal encryption algorithms are the pedo-terrorists then they will become easier to track and spot. This argument is valid, however there are many ways to hide both online and offline which are outside the scope of this discussion. The point remains that strong encryption will be available to the people that want to use it.

Eventually, over time, weaknesses will be found in our current best-of-the-best encryption schemes. If the government would have its way then there would be no new publicly available strong encryption algorithms because research in that area would be either illegal or at least unfunded. However, that doesn't mean that strong encryption will not exist. Strong encryption is math and math cannot be killed, banned or mutated. A historical example is [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) which was created by Phil Zimmermann so that activist could communicate securely over BBSs. Soon after PGP's release the US Government opened an investigation over "munitions export without a license" because export laws at the time considered encryption software munitions. In response activists published the sourcecode to PGP in a hardback book which couldn't be legally controlled under the munitions exports laws. It didn't take long for PGP to spread around the world and is still today one of the best encryption mechanisms around.

## Removing Strong Encryption From the Public Only Makes the Public Vulnerable

Banning strong encryption and discouraging its use will set a privacy-killing precedent that will be extremely hard to undo. If the US government (The "good" guys if you are in the US, the "bad" guys if you are in China) mandates that all its constituents must use "Magic Backdoor Encryption" then the Chinese government (The "bad" guys according to the US, and the "good" guys as far as China is concerned) would be completely reasonable in demanding the same. Soon enough every government will require all of its constituents to use "Magic Backdoor Encryption" to which it holds the keys. If you are a multi-national corporation then you might end up in a pickle where you have to use "Magic Backdoor Encryption" with keys that both the US and China, along with every other nation, can break at will.

After governments mandate "Magic Backdoor Encryption" for "national security" reasons, the practice will become culturally acceptable and people will be used to the idea that their secrets are not really secret. Soon after, corporations will start demanding that its employees use "Magic Backdoor Encryption" for which it holds they keys under the excuse of preventing fraud/leaks/embezzlement etc. Once "Magic Backdoor Encryption" becomes a standard in corporations, parents will demand devices with "Magic Backdoor Encryption" that allow them to decrypt their children's messages "to protect the children", and suddenly encryption losses all of it's current meaning.

## Arguments to Why Law Enforcement Should Be Able to Crack Encryption

The discussion of whether law enforcement should be able to break encryption, even when provided with a search warrant, is not just a technical discussion, it is also a policy and civil liberties discussion.

It is important to keep in mind that we, as a government by the people and for the people, are as a society responsible for both creating the law and enforcing the law. Being the law there are moments where we, as a society, have had the right to break the law and have exercised that right. During the prohibition era the people and population found the law illegitimate and therefore willingly and knowingly broke the law. Without the people's ability to willingly and knowingly resist prohibition the law would still be force today.

A more modern example is marijuana legalization. Personal opinions aside, the societies in certain states have decided that the marijuana prohibition laws were not reasonable, despite the governments good faith, and chose to repeal those law. Without the ability and freedom to break those laws, protected by the privacy of their homes, the people of those states would not have been able to gain support and repeal what they saw as unreasonable laws.

The encryption debate is less about technology and more about personal privacy and government surveillance. By giving up our personal privacy and giving law enforcement the ability to watch our every movement, discussion and opinion once we are only *suspected* of breaking a law, we are forgoing a large portion of our liberties and one of our basic human rights, namely the right to privacy.

### It Will Help Solve Some Cases

The obvious reason that the government and law enforcement would want to break encryption is that it will provide them access to data that might be beneficial to "national security" or solving a case. This is undeniably true. But at the same time it is not a sufficient reason to either outlaw encryption nor a reason to deprive people of the security it provides. We as a society don't outlaw paper shredders because they could be used by terrorists to destroy evidence and we don't outlaw gloves and ski masks because they can be used to hide fingerprints and faces. Encryption, just like paper shredders, gloves and ski masks, is a tool that in itself is neither good nor evil, but it can be used for either good or evil.

### I Didn't Do Anything Wrong so I Have Nothing to Hide

> Arguing that you don't care about the right to privacy because you have nothing to hide is no different than saying you don't care about free speech because you have nothing to say
> <cite>Edward Snowden</cite>

There are two issues with the "I have nothing to hide" argument. The first issue is very simple to understand if you do not assume that the government is always good. One doesn't have to look much farther then the numerous dictatorships around the world to see that loosing one's privacy under a repressive regime puts a person at risk of exploitation and oppression. In fact stripping the population of it's privacy and peoples trust of one another is one of the first rules in the oppressors handbook. If we give up our privacy now, at a time when we trust and believe the government is acting in good faith, we will never be able to gain it back in the future when our government and private corporations turn against us and we would have provided the government the cornerstone for building a dictatorship should it choose to do so.

Even if one believes that his/her government is good, or at least acting in good faith, and will continue to be benevolent for the indefinite future, there are still strong reasons to protect personal privacy. As stated in [Privacy and Human Rights](http://gilc.org/privacy/survey/intro.html), *privacy is a fundamental human right and underpins human dignity, freedom of association and freedom of speech*. In the prohibition examples stated above, none of the reforms would be possible without the right to privacy, even though the government and law enforcement were working with good intent.

Because the current violations of personal privacy are distant and invisible to most people, they do not see or feel that their privacy has been violated. However the current privacy violations are no different then having a company require you hand over your unlocked phone every time you enter the building and allow them to comb through it, or having to list every person you have communicated with every time you enter a government building. We would not accept these violations in the physical world, but in the digital realm this is the world we have created and these practices remain unchallenged because they are unseen thus absent from the mind.

## Conclusions

The discussion of weakening encryption to further the governments and law enforcements purposes is not only a technical debate, but is also a social, political and civil liberties debate we must all be deeply concerned about. Furthermore the technical aspects of encryption make weakening encryption an impractical and temporary solution for "catching the bad guys" while eroding our personal right and civil liberties for very little return.
