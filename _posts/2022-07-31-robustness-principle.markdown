---
layout: post
title: "Postel's Law: Protocol Disintegration By Committee"
tags: protocol
---

There's a thoroughly written rule within software communities that you may
have come up against: *don't implement the protocol yourself.*

I was about 15 when I was first faced with this refrain; I was an arrogant
teenager who insisted that even if the wheel had already been invented, I
wanted to take my swing at it; maybe my wheel would be wheelier than theirs,
maybe I'd learn something about wheels, maybe I'd have fun. I wanted to
viscerally understand the machinations of the code that sits behind the
curtain of human-friendly interfaces, as if doing so would make me a *real*
programmer.

I've grown a love-hate relationship for that partially-enduring impulse; I get
a deep satisfaction from cresting the mountain of a learning curve, but I also
manage to scope-creep any project I get my paws on, and what I've learned
along the way is that the majority of psychic damage inflicted by
reimplementing widely-implemented protocols is just how bad other people's
implementations can be.

## *you'd think they could at least get that right??*

```
/* Stupid broken piece of shit ircd didn't send us 001,
   you'd think they could at least get that right??
   But no, then I'll have to go and add these idiotic kludges
   to make them work. Maybe I should instead get the users of these
   servers to complain about it to their admins.
   
   [...] */
```

[This][00] understandably disgruntled comment clearly expresses the dilemma
here: you either begrudgingly handle other software's incorrect data, or you
hope that ecosystemic pressure may force their hand to stop doing it. I'm here
to tell you this developer made the wrong choice, and how this choice
consistently facilitates protocol degradation that makes implementing
protocols an insurmountable task.

## *Unicode, Dammit*

A common casualty of permissive input handling is [anything][01] that handles
web pages. 

Any of you that have had the *joy* of being in the weeds of web development
will be all-too-familiar with how frequently *it works on my machine* isn't a
sufficient answer; there's a [whole industry][02] built around one of the
pains that the robustness principle promulgates; browsers have all had to make
their own secret sauce for handling the many innovative ways people have found
to write web pages incorrectly, and you cannot trust that your code is correct
just because your browser makes it look ok.

People are, by and large, free to write absolutely god-foresaken HTML that,
when you [squint][03], looks roughly logical, so browsers help you out and
take a guess at what you actually meant, rather than telling you that you've
written something invalid and really ought to go figure out how. The fact that
people don't even know they've done something incorrect hurts the ecosystem,
skips over an opportunity for that person to learn, and makes the task of
implementing generalised web page parsing out of the reach of most people.

HTML5 has [spec-defined][04] the *correct* way to handle invalid HTML
documents which may help this; it's still going to allow incorrect code to
proliferate unimpeded, but at least it will be somewhat predictable. There's a
lot of reasons that the browser market [isn't a crowded field][05], but I'm
willing to bet the bar to entry of handling global decades worth of
noncompliant data doesn't help.

## *I have allowed it anyway.*

While doing a bit of research for this blogpost, I happened upon a
[comment][06] in pyyaml's code that sent me down a bit of a rabbit hole;

```
# For some strange reasons, the specification does not allow '_' in
# tag handles. I have allowed it anyway.
```

'Tags' are a way to decorate a value with a hint to tell the parser how to
parse it. think of an example like `date: !iso8601 2022-01-01`; `!iso8601` is
the tag, `iso8601` is the tag handle.

As promised, [the specification][07] does not allow `_` in tag handles, but
the [earliest commit][08] of tag handles in libyaml does; this commit does come
after the publication of yaml 1.1, but that [also][09] does not allow `_`, and
neither does the [most recent version][10] of the yaml specification. A
popular [rust library][11] accepts `_`, as does a popular [java library][12],
as does a popular [.net][13] library, et cetera.

Although the case of tag handles are a bit of a niche (just happened to have
the best comment), what this demonstrates is that for anyone optimistic enough
to try their hand at implementing this protocol, reading and implementing the
specification alone will not suffice for functionality parity with even the
reference implementations of it. They will come to learn that, no matter what
the specification says, someone will end up approaching your software with
data they expect to work, because the reference implementation and most
implementations derived from it will accept it.

## Why does this matter?

Something that motivated me to write this blogpost is my sorrow at communities
moving from IRC to proprietary communication platforms simply because they're
nicer to use, and the upsetting realisation that a massive barrier to entry
for writing new and better IRC software is how absolutely maddening it is to
try to write IRC software capable of accepting 3 decades worth of
[infamously][15] subtly invalid protocol being spoken by other software.

As the complexities of writing software grow, the accessibility for hobbyists
and enthusiasts wanes. We're watching the industry creep further and further
towards meaningful projects needing to be backed by well-resourced outfits to
be able to find their feet and that's intensely damaging for personal
freedoms, as well turning away would-be revolutionaries from even trying to
make their mark. Something that is definitely within our collective control is
ensuring our own software contributes to enforcing spec-compliance, so that
new software starts off having to compare itself to a well-functioning
ecosystem.

I do recognise that reversing the dogma of permissive input handling is going
to be impractical for a lot of already-broken protocols without very slow
incremental moves like HTML5 has tried, but it feels like we as free software
advocates should have a moral compulsion to keep protocols reasonable to
implement to avoid concentrating expertise in to precariously few hands, and
The Robustness Principle, practically, runs counter to this goal.

[00]: https://github.com/irssi/irssi/blob/de46fee864818c8174aa63342378f18fb686ea72/src/irc/core/irc-servers.c#L1051-L1058
[01]: https://bazaar.launchpad.net/~leonardr/beautifulsoup/bs4/view/642/bs4/dammit.py
[02]: https://www.browserstack.com/
[03]: https://bazaar.launchpad.net/~leonardr/beautifulsoup/bs4/view/642/bs4/builder/_htmlparser.py#L83
[04]: https://html.spec.whatwg.org/multipage/parsing.html#parse-errors
[05]: https://en.wikipedia.org/wiki/Comparison_of_browser_engines
[06]: https://github.com/yaml/pyyaml/blob/0abad85a17ba75c0fb431feea7a6a06125341a99/lib/yaml/scanner.py#L1350-L1351
[07]: https://yaml.org/spec/1.0/#ns-tag-char
[08]: https://github.com/yaml/libyaml/blob/e71095e3bf9b5f2222cde446327506542f86c847/src/scanner.c#L2951
[09]: https://yaml.org/spec/1.1/#ns-word-char
[10]: https://yaml.org/spec/1.2.2/#rule-ns-word-char
[11]: https://github.com/chyh1990/yaml-rust/blob/da52a68615f2ecdd6b7e4567019f280c433c1521/src/scanner.rs#L793
[12]: https://bitbucket.org/snakeyaml/snakeyaml/src/607672097f3aeec3357488881440c730900b06b6/src/main/java/org/yaml/snakeyaml/scanner/ScannerImpl.java#lines-2228
[13]: https://github.com/aaubry/YamlDotNet/blob/fd6731d4ab6bb4f3bec79a47b9ecb60f0d8e415f/YamlDotNet/Core/Scanner.cs#L2550
[14]: https://github.com/nodeca/js-yaml/blob/49baadd52af887d2991e2c39a6639baa56d6c71b/lib/loader.js#L28
[15]: https://modern.ircdocs.horse/
