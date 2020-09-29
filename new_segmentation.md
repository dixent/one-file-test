# 💻📖 hacker-laws

Laws, Theories, Principles and Patterns that developers will find useful.

[Translations](#translations): [🇧🇷](./translations/pt-BR.md) [🇨🇳](https://github.com/nusr/hacker-laws-zh) [🇩🇪](./translations/de.md) [🇫🇷](./translations/fr.md) [🇬🇷](./translations/el.md) [🇮🇹](https://github.com/csparpa/hacker-laws-it) [🇱🇻](./translations/lv.md) [🇰🇷](https://github.com/codeanddonuts/hacker-laws-kr) [🇷🇺](https://github.com/solarrust/hacker-laws) [🇪🇸](./translations/es-ES.md) [🇹🇷](https://github.com/umutphp/hacker-laws-tr) [🇯🇵](./translations/jp.md)

Like this project? Please considering [sponsoring me](https://github.com/sponsors/dwmkerr) and the [translators](#translations).

---

<!-- vim-markdown-toc GFM -->

* [Introduction](#introduction)
* [Laws](#laws)
    * [90–9–1 Principle (1% Rule)](#9091-principle-1-rule)
    * [Amdahl's Law](#amdahls-law)
    * [The Broken Windows Theory](#the-broken-windows-theory)
    * [Brooks' Law](#brooks-law)
    * [Conway's Law](#conways-law)
    * [Cunningham's Law](#cunninghams-law)
    * [Dunbar's Number](#dunbars-number)
    * [Gall's Law](#galls-law)
    * [Goodhart's Law](#goodharts-law)
    * [Hanlon's Razor](#hanlons-razor)
    * [Hofstadter's Law](#hofstadters-law)
    * [Hutber's Law](#hutbers-law)
    * [The Hype Cycle & Amara's Law](#the-hype-cycle--amaras-law)
    * [Hyrum's Law (The Law of Implicit Interfaces)](#hyrums-law-the-law-of-implicit-interfaces)
    * [Kernighan's Law](#kernighans-law)
    * [Metcalfe's Law](#metcalfes-law)
    * [Moore's Law](#moores-law)
    * [Murphy's Law / Sod's Law](#murphys-law--sods-law)
    * [Occam's Razor](#occams-razor)
    * [Parkinson's Law](#parkinsons-law)
    * [Premature Optimization Effect](#premature-optimization-effect)
    * [Putt's Law](#putts-law)
    * [Reed's Law](#reeds-law)
    * [The Law of Conservation of Complexity (Tesler's Law)](#the-law-of-conservation-of-complexity-teslers-law)
    * [The Law of Demeter](#the-law-of-demeter)
    * [The Law of Leaky Abstractions](#the-law-of-leaky-abstractions)
    * [The Law of Triviality](#the-law-of-triviality)
    * [The Unix Philosophy](#the-unix-philosophy)
    * [The Spotify Model](#the-spotify-model)
    * [Wadler's Law](#wadlers-law)
    * [Wheaton's Law](#wheatons-law)
* [Principles](#principles)
    * [The Dilbert Principle](#the-dilbert-principle)
    * [The Pareto Principle (The 80/20 Rule)](#the-pareto-principle-the-8020-rule)
    * [The Peter Principle](#the-peter-principle)
    * [The Robustness Principle (Postel's Law)](#the-robustness-principle-postels-law)
    * [SOLID](#solid)
    * [The Single Responsibility Principle](#the-single-responsibility-principle)
    * [The Open/Closed Principle](#the-openclosed-principle)
    * [The Liskov Substitution Principle](#the-liskov-substitution-principle)
    * [The Interface Segregation Principle](#the-interface-segregation-principle)
    * [The Dependency Inversion Principle](#the-dependency-inversion-principle)
    * [The DRY Principle](#the-dry-principle)
    * [The KISS principle](#the-kiss-principle)
    * [YAGNI](#yagni)
    * [The Fallacies of Distributed Computing](#the-fallacies-of-distributed-computing)
* [Reading List](#reading-list)
* [Translations](#translations)
* [Related Projects](#related-projects)
* [Contributing](#contributing)
* [TODO](#todo)

<!-- vim-markdown-toc -->

## Introduction

There are lots of laws which people discuss when talking about development. This repository is a reference and overview of some of the most common ones. Please share and submit PRs!

❗: This repo contains an explanation of some laws, principles and patterns, but does not _advocate_ for any of them. Whether they should be applied will always be a matter of debate, and greatly dependent on what you are working on.

## Laws

And here we go!

### 90–9–1 Principle (1% Rule)

[1% Rule on Wikipedia](https://en.wikipedia.org/wiki/1%25_rule_(Internet_culture))

The 90-9-1 principle suggests that within an internet community such as a wiki, 90% of participants only consume content, 9% edit or modify content and 1% of participants add content.

Real-world examples:

- A 2014 study of four digital health social networks found the top 1% created 73% of posts, the next 9% accounted for an average of ~25% and the remaining 90% accounted for an average of 2% ([Reference](https://www.jmir.org/2014/2/e33/))

See Also:

- [Pareto principle](#the-pareto-principle-the-8020-rule)

### Amdahl's Law

[Amdahl's Law on Wikipedia](https://en.wikipedia.org/wiki/Amdahl%27s_law)

> Amdahl's Law is a formula which shows the _potential speedup_ of a computational task which can be achieved by increasing the resources of a system. Normally used in parallel computing, it can predict the actual benefit of increasing the number of processors, which is limited by the parallelisability of the program.

Best illustrated with an example. If a program is made up of two parts, part A, which must be executed by a single processor, and part B, which can be parallelised, then we see that adding multiple processors to the system executing the program can only have a limited benefit. It can potentially greatly improve the speed of part B - but the speed of part A will remain unchanged.

The diagram below shows some examples of potential improvements in speed:

<img width="480px" alt="Diagram: Amdahl's Law" src="./images/amdahls_law.png" />

*(Image Reference: By Daniels220 at English Wikipedia, Creative Commons Attribution-Share Alike 3.0 Unported, https://en.wikipedia.org/wiki/File:AmdahlsLaw.svg)*

As can be seen, even a program which is 50% parallelisable will benefit very little beyond 10 processing units, whereas a program which is 95% parallelisable can still achieve significant speed improvements with over a thousand processing units.

### The Broken Windows Theory

[The Broken Windows Theory on Wikipedia](https://en.wikipedia.org/wiki/Broken_windows_theory)

The Broken Windows Theory suggests that visible signs of crime (or lack of care of an environment) lead to further and more serious crimes (or further deterioration of the environment).

This theory has been applied to software development, suggesting that poor quality code (or [Technical Debt](#TODO)) can lead to a perception that efforts to improve quality may be ignored or undervalued, thus leading to further poor quality code. This effect cascades leading to a great decrease in quality over time.

See also:

- [Technical Debt](#TODO)

Examples:

- [The Pragmatic Programming: Software Entropy](https://pragprog.com/the-pragmatic-programmer/extracts/software-entropy)
- [Coding Horror: The Broken Window Theory](https://blog.codinghorror.com/the-broken-window-theory/)
- [OpenSource: Joy of Programming - The Broken Window Theory](https://opensourceforu.com/2011/05/joy-of-programming-broken-window-theory/)

### Brooks' Law

[Brooks' Law on Wikipedia](https://en.wikipedia.org/wiki/Brooks%27s_law)

> Adding human resources to a late software development project makes it later.

This law suggests that in many cases, attempting to accelerate the delivery of a project which is already late, by adding more people, will make the delivery even later. Brooks is clear that this is an over-simplification, however, the general reasoning is that given the ramp up time of new resources and the communication overheads, in the immediate short-term velocity decreases. Also, many tasks may not be divisible, i.e. easily distributed between more resources, meaning the potential velocity increase is also lower.

The common phrase in delivery "Nine women can't make a baby in one month" relates to Brooks' Law, in particular, the fact that some kinds of work are not divisible or parallelisable.
