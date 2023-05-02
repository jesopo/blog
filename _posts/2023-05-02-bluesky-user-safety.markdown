---
layout: post
title: "Bluesky's user safety situation"
tags:
  - bluesky
  - moderation
---

The new kid on the block for microblogging is a platform called [Bluesky][1],
and I've been thinking about how user safety intersects with its unique
circumstances.

I won't be occupying myself with whether this platform or the [protocol][2] it
is built on fills a niche not already filled by other options. It exists,
people are using it, and we're going to need to have frank conversations about
user safety on it regardless.

I'll lay my cards on the table early. I'm optimistic about Bluesky and the
motivations of the team working on it, but I'm also realistic. All I really
have to go on is the documentation provided by Bluesky so far and their stated
intent for the future, but they could change their minds on any of it.

Throughout this post, I will be discussing the good and bad parts of Bluesky,
both as the invite-only walled garden it currently is, and as the federated
platform it promises to be.

## Conceptual brass tacks: good things, difficult things, and my suggestions

### Not much metadata on remote users

There's an extremely glaring downside to abuse mitigation capabilities in
decentralised social systems. Centralised systems like Twitter can benefit
from user metadata when detecting ban evasion; user agents, source IP
addresses, choice of email provider, etc. But in a decentralised network, you
do not have this metadata for users on other homeservers. What this
practically means is you will need to end up using the metadata you _do_ have,
such as what homeserver a user is federating through, how they choose their
handles, who they interact with, etc.

The way [Mastodon][3] and [Matrix][4] tend to handle this shortcoming is
defederating homeservers that seem to be the source of many abusive users. It
ends up being a game of "if you don't keep your house in order, we will ignore
your house." This works, and I hope Bluesky ends up leaning on this, and I
hope they end up surfacing the ability to mute/block all users from a given
homeserver to end-users as well as homeserver admins. The one problem with
this methodology on Bluesky (compared to Mastodon) is moving your account to a
new homeserver has almost no friction. The way I foresee solving this is
something like "mute every user whose account was initially registered on a
given bad homeserver."

Corollary to the above: if there's a homeserver that becomes too big to
defederate, like mastodon.social is on Mastodon, they **must** ensure their
users are roughly reputable. If a homeserver is too big to defederate and
they're a source of a lot of abusive users, the network will become unusable.

### Keeping your own house in order

In existing federated networks, like email, Mastodon, and Matrix, homeservers
function as a user's entrypoint to a network. There's far fewer homeservers
than there are users, and homeservers are the only people that know all of the
possible metadata about the clients transiting data through them. This means
that you, a homeserver admin or an end user can pass judgement on the quality
of remote homeserver's users by empirical evidence, decide that they are not
doing enough work to prevent people transiting abuse through them, and then
reject everything coming from them.

Abusive users _can_ find new homeservers to pass their abuse through, and you
_will_ play a game of cat and mouse to weed bad homeservers from your view of
the network, but email has been doing this for decades, and it _mostly_ works.

This is only necessary for homeservers that are very permissive with new
registrations. A homeserver can ban a remote user without deferating the whole
homeserver but if a given user is repeatedly making new accounts on a remote
homeserver, you will **need** to treat all users of that homeserver with
suspicion. Something I would urge people to keep in mind in this area is
there's no technical limitation stopping you from saying "mute any user from
homeserver $foo that I have not already seen before."

Homeservers **must** employ baseline industry-standard methodologies for
pruning abuse from themselves if signups to them are open to the world or face
ostracism. Monitor signups, prevent the most obvious cases of registering many
different accounts for abuse, detect behaviour that looks like spam, etc.

### Batteries need to be included

The Bluesky team have, encouragingly, been preaching about giving users the
tools needed to carefully curate what they see on Bluesky by use of
[first-class support for custom feed algorithms][5]. This is a good idea, but
it needs to be implemented with the understanding that if new users see nazis
all over Bluesky when they don't yet have custom algorithms, Bluesky will have
terrible retention of new users.

The way Mastodon approaches this is your homeserver admins lay down a baseline
of content curation by defederating known bad homeservers on the network. An
advantage Bluesky has in this situation is they're happy for the bsky.social
homeserver to be the common entrypoint to the network for new users, so they
have a lot of power to shape the first view of the network that new users get.

### Invite tree reputation

Bluesky is currently invite-only and the dev team are leaning hard on this to
stem the flow of abusive users. This methodology is surprisingly effective:
you can find the people inviting the people that are abusing other users and
prevent from them inviting more people. Private torrent trackers have been
relying on this for decades. The (small) problem with this is that you cannot
be sure that the 54k people you currently have are handing out their invites
wisely. A sufficiently motivated abusive user may prove adept at finding new
ways in, especially when invites are being given to existing users so readily.

Once federation lands, this is of limited use. You can use this methodology to
keep your own house in order, and encourage other homeservers to use it to
keep their houses in order, but this does nothing to protect your users from
remote users on homeservers that do not use this. Even if a remote homeserver
were to try to communicate their invite tree to you (**bad idea!**), they can
simply lie about their invite trees.

## Current real problems

### It's still in beta

Some using the platform now are using it as if every _important_ detail has
been worked out, and I can understand why. At the time of writing, there's
just over 54k users, including some very big names. The fact that some big
things are still being worked out is not always glaringly obvious.

Notably, it was only a few days ago that the Bluesky dev team had to drop
everything they were doing to land the ability to [block other users][6]. This
feature was implemented with a detail (that blocks are publicly viewable data,
more on this later) that has been widely criticised. I've also been led to
believe that the ability to deactivate accounts also had to be quickly
implemented in response to someone needing to be deactivated. Whether or not
this is true, it's definitely true that they're building a plane's engine
while the plane is hurtling through the sky and they're quickly growing to a
size where user safety is becoming urgent.

Other shortcomings include; muted words are not supported, locked accounts are
missing and might take quite a while to implement (for technical reasons), you
cannot hide replies, reporting categories are limited, etc, etc.

### Speech vs Reach

Something we've also seen [Twitter say][7] a number of times recently is that
harmful content can (and should) be allowed to exist but should have its
ability to reach other users limited. It seems the brunt of the argument in
favour of this is that freedom of speech means people should be allowed to say
whatever they want but Twitter as a platform doesn't have to help deliver
everything you say to a wide audience. Looking at what [documentation][8] is
available and reading things the Bluesky devs have said makes it apparent that
they plan to lean into this angle too.

![image]({{ site.baseurl }}/assets/img/1a5875f17ade6c96c35a1db6acb589f6389ae427.png)

While the above does seem promising, the wording of it does leave something to
be desired. It's conjecture, but my feeling so far is that Bluesky would like
to manage as many things as possible only by limiting reach, and only resort
to things like account deactivation in extenuating circumstances. Please,
**keep your own house in order.**

**This being said**, there's a big difference between users posting bad
opinions, and accounts posting opinions that, when taken to their inevitable
conclusion, amount to fascism, or users that are going out of their way to
harm the mental health of other users, especially those in at-risk
demographics. This is a hard balance to strike; sometimes it is hard to know
what should be considered reasonable debate, and what should be considered
beyond the pale. Hire people that know this difference.

This topic, like many others here, is complicated by federation. If a remote
user is being a bastard, you have two options: refuse all content from the
user, or label content from the user based on what bastardy thing it is. You,
as a homeserver admin, can opt to hide the labeled content for your users, or
you can leave it up to your users. It is currently unclear what the
bsky.social homeserver plans to block for you and what it plans to leave up to
you.

You, as a homeserver admin, can't opt to simply refuse bastard posts from a
remote user. You can only validate the authenticity of a given post by knowing
the posts from the author that came before it, so if you refuse one post, you
can no longer validate any posts that come after it. This doesn't practically
matter in most cases: if you label their bastardy posts and don't show them to
users, that's mostly fine. A problem occurs if a user posts something illegal
that you cannot tolerate persisting anywhere, but their feed is otherwise fine
(imagine their account is briefly compromised, or so.)

![image]({{ site.baseurl }}/assets/img/ef8a0aac4f3eef1a23d3e0873f617494515761ff.png)

I am hoping that bsky.social intends to deactivate accounts using bsky.social
as their homeserver if they fall into the "Political Hate-Groups" category,
but there has thus far been some instances of things like harassment,
transphobia, and racism that have not been acted on as fast as one might hope,
and I hope this is a symptom of problems happening faster than they can be
solved rather than a deliberate choice of "limiting reach."

On the off chance that this is a case of "limiting reach" as a means to avoid
more forceful actions, I'd like to make a few points about the limitations of
that methodology:

#### _"Speech means nothing if no one can hear you!"_

Making a distinction between freedom of speech and freedom of reach seems
dubious at best. If you force a racist into a room on their own so no one can
hear them, they'll inevitably argue that they are not being afforded freedom
of speech, so this doesn't feel like it helps at all.

#### Freedom of speech is for governments, not social media

The goals of freedom of speech were, and should be, only the freedom to
criticise your government without fear of retribution.

I come from Europe which, by and large, understands that freedom of speech
does not extend to you being able to intentionally offend people without
consequence, but is a backstop designed to prevent a government imprisoning
you for disagreeing with them. It seems to me that The United States of
America has in recent times dominated international discourse about this
topic, where many believe that you should, in fact, be allowed to be racist
without facing consequences.

There's definitely a lot of nuance here. If a government bought in draconian
surveillance to detect their citizens being racist, you really should be
afforded protections enough to criticise such a policy but you can very easily
criticise such a policy without yourself being vocally racist.

#### People will find the content anyway
Anything short of making that content entirely inaccessible will mean people
will eventually find it. This will contribute to your platform _feeling_
unsafe to certain people, and will likely lead (rightly or wrongly) to a
[nazi bar][9] PR problem.

You can make all the technical and philosophical explanations you want; if
people don't feel comfortable knowing there's nazis in the same room as them,
"but you can't see them" isn't a particularly potent salve.

### Algorithms all the way down

![image]({{ site.baseurl }}/assets/img/034fd1befcc3e4ced4f91bd182a34378705fe8eb.png)

So far it seems that a number of problems in this sphere have been met with
"we can solve that with AI-automated content labeling", or "you'll be able to
curate your feed with custom algorithms!"

![image]({{ site.baseurl }}/assets/img/35ed930ebe58a83e34b3ebe32c9c128487a4a4b3.png)

Again, I can understand this refrain, but I'm unconvinced that this Technology
is developed enough to sufficiently handle user safety in the medium term, and
I'm acutely aware that a future plan for algorithms and AI does not solve the
fact that there are blossoming problems _right now_. People in at-risk
demographics are currently using Bluesky quite heavily.

![image]({{ site.baseurl }}/assets/img/09a4dfbaa7cf2d7bf9c47847230d13c598599198.png)

AI-automated content recognition, in the current real world, can only
practically be relied upon to pick low hanging fruit. You are going to need a
thorough and well-tooled team of humans handling everything else, and while I
respect that the dev team has been able to do so much with so few people, I
can feel that an inflection point is **rapidly** approaching.

### Almost everything is public

![image]({{ site.baseurl }}/assets/img/52319dc212b2c28fc6d684c0b0400c2c147405dc.png)

In the last few days Bluesky had to urgently implement the ability to block
users. The app launched with the ability mute other users but not the ability
to block other users. The key difference between blocking and muting is that
blocked users will know they are blocked, and their clients can attempt to
prevent them from composing posts that mention or reply to the user that
blocked them. Additionally, mutes only hide notifications from muted users,
while blocking will also hide the blocked user from timelines, but this is an
implementation detail that **could** be changed.

The explanation (to the best of my knowledge) for why blocks are public is
twofold: Bluesky plans to be a federated network, so a remote user's
homeserver needs to know if they're blocked or not, but also if users can
seamlessly move from one homeserver to another the new homeserver also needs
to be cognizant of blocks. If I block you while I'm using bsky.social but then
you move to jesopo.uk, it'd not be great for you to magically become
unblocked. Corollary; if I block you while you're on bsky.social, bsky.social
will tell you that you can't mention me, and it would not be great if you
could then move to jesopo.uk, without having to make a whole new account and
social graph, and suddenly be freely permitted to mention me, even if my
homeserver will still not show me you mentioning me.

Mutes on the other hand are not public; they solely function to hide the muted
user's content from the muter. I'm unconvinced that the explanation for blocks
being public is convincing enough that I'm willing to overlook how damaging
public block lists can be to people. You could argue that given the target of
your block on Twitter can already see that you've blocked them, they can
already send a dogpile after you, but this does make that easier.

Additionally, as currently implemented; every post, follow, like, etc is
accessible from unauthenticated APIs, and every change to publicly accessible
information anywhere on the platform is able to be streamed through an
unauthenticated websocket endpoint on the API. One might argue that this is
necessary for federation, but the [docs disagree][10]. From a purely
theoretical standpoint, one _could_ do what mastodon does: only inform a
remote homeserver about a new post you make if they have a user that follows
you.

This all leaves Bluesky wide open to mass data archival without user consent.
This is a topic Mastodon has been struggling with a lot and they have opted
to (mostly) solve it by [Authorized Fetch][11] which requires an entity
requesting a record to identify themselves. Some may say "if you post
something publicly, you consent to it being archived!" but many, myself
included, believe there's more than just public and private content. Sometimes
you want to be findable but not have the spotlight on you. If you are a public
figure, assuming anything you post in public is going to be archived makes
sense, but if you're just some random user trying to have a good time, it can
feel like an invasion of privacy.

### Deletes are git reverts, at the moment

![image]({{ site.baseurl }}/assets/img/79aee9442f4aef11ede0a0a3be8ccb9f4e949b03.png)

When you delete a post, or undo a follow, or undo a like, etc etc it currently
simply adds a "I've deleted that" event to your repository. If someone knows
where to look, **they can find deleted records**.

Consider your entire user is a git repository, and each record type you can
make (post, like, follow, etc) is a directory in your git repo. When you add a
record, it makes a commit to your repo adding that record. Each commit's
validity, like git, relies on knowing the thing that came before it. This
means that, like a git repo, you need to rewrite the entire commit log from
the deleted commit onwards to actually delete a commit. Bluesky does not
currently expose the ability to rebase your repo:

![image]({{ site.baseurl }}/assets/img/897533d8791f2679780baf06fdad0164e99bbd53.png)

I'm very glad to hear that they intend to expose this functionality but it's
definitely a real issue to be really concerned about right now.

### Soft blocks can never be supported

To follow someone, you create a "follow" record in your repository. The only
person that can revoke that record is whoever holds the private key for your
repo. If another user blocks you and you're currently following them, the
block event would have to ask you to revoke your own follow.

Since federation is not yet enabled and since Bluesky (at least currently)
has your private key, they _could_ action an unfollow for you when you get
blocked, but this doesn't reliably hold true in a federated world, nor in a
potential world where clients are the only holders of their private keys,
where a homeserver could purely exist to mirror your data on to the wider
network. Even in the circumstances where this could work, it doesn't seem
right to sign a piece of data without explicit instruction from the user to
which the private key belongs.

This doesn't mean that if someone is following you and you block them, they'll
still see your posts. They won't. On a well-behaved homeserver it will not
show your posts to blocked users even if they're following you, and Bluesky
**could** be implemented such that a remote user's homeserver isn't told about
your new posts if they don't have any non-blocked users following you.

I'm unsure how practical this would be, but the protocol _could_ be changed
such that every attempt to follow a user needs to be co-signed by the user
being followed. That user can then revoke their co-signature when they block
the follower, though this would mean that resolving a user's following list
would involve cross-referencing it with co-signatures to check for revocation.
I have a feeling something like this is going to need to be implemented
anyway if Bluesky plans to have locked accounts some day.

### Misleading URLs and mentions

![image]({{ site.baseurl }}/assets/img/261f7a3c564e5c4ef632f8199d75f4b7a0a11d4e.png)

It is currently extremely trivial to create URLs that look like something
they're not. it's very, very easy to make a "google.com" post that actually
leads to a RickRoll (or much, much worse) or make a mention that, when
clicked, leads to a very different user than you'd expect. The URL problem
here is much more of a concern. This problem is solvable to a degree, and I've
seen the devs musing on how they'd fix it, which is promising.

## Conclusions

I am mostly hopeful for the future, but I am also concerned about the
challenges posed by the relatively-new frontier that is moderation on a
federated network. I think these challenges may have good solutions, but will
need time, practice, and experimentation.

I also think the Bluesky devs might have bitten off more than they can chew in
the short term. The devs do seem to accept this. Their team is small, they're
working very hard, and their intentions seem good; but the last week has seen
a large influx of users, as well as an influx of problems that require both
careful consideration and better tools.

In the longer term, I'm encouraged by a stated goal of
[Composable Moderation][12], and I'm encouraged by the idea of shared
reputation lists that users can subscribe to and publish, to shape their own
view of the network and help others shape their own views of the network, but
I think the responsibilities of a homeserver admin are still going to include
protecting their users using a combination of traditional methodologies as
well as the brave new world of moderation that can be afforded to us all with
decentralisation and algorithms.

[1]: https://blueskyweb.xyz/
[2]: https://atproto.com/
[3]: https://joinmastodon.org/
[4]: https://matrix.org
[5]: https://blueskyweb.xyz/blog/3-30-2023-algorithmic-choice
[6]: https://github.com/bluesky-social/atproto/pull/922
[7]: https://twitter.com/elonmusk/status/1598752139278532610
[8]: https://atproto.com/guides/overview#speech-reach-and-moderation
[9]: https://en.wiktionary.org/wiki/Nazi_bar
[10]: https://atproto.com/guides/overview#achieving-scale
[11]: https://docs.joinmastodon.org/admin/config/#authorized_fetch
[12]: https://blueskyweb.xyz/blog/4-13-2023-moderation
