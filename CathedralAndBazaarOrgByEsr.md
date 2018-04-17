# The Cathedral and the Bazaar

**Eric Steven Raymond**

**Copyright:** Permission is granted to copy, distribute and/or modify this document under the terms of the
Open Publication License, version 2.0.

**Abstract**

I anatomize a successful open-source project, fetchmail, that was run as a deliberate test of the surprising
theories about software engineering suggested by the history of Linux. I discuss these theories in terms of
two fundamentally different development styles, the ''cathedral'' model of most of the commercial world
versus the ''bazaar'' model of the Linux world. I show that these models derive from opposing assumptions
about the nature of the software-debugging task. I then make a sustained argument from the Linux
experience for the proposition that ''Given enough eyeballs, all bugs are shallow'', suggest productive
analogies with other self-correcting systems of selfish agents, and conclude with some exploration of the
implications of this insight for the future of software.

**Table of Contents**

The Cathedral and the Bazaar

The Mail Must Get Through

The Importance of Having Users

Release Early, Release Often

How Many Eyeballs Tame Complexity

When Is a Rose Not a Rose?

Popclient becomes Fetchmail

Fetchmail Grows Up

A Few More Lessons from Fetchmail

Necessary Preconditions for the Bazaar Style

The Social Context of Open-Source Software


## The Cathedral and the Bazaar

Linux is subversive. Who would have thought even five years ago (1991) that a world-class operating system
could coalesce as if by magic out of part-time hacking by several thousand developers scattered all over
the planet, connected only by the tenuous strands of the Internet?

Certainly not I. By the time Linux swam onto my radar screen in early 1993, I had already been involved in
Unix and open-source development for ten years. I was one of the first GNU contributors in the mid-1980s.
I had released a good deal of open-source software onto the net, developing or co-developing several
programs (nethack, Emacs's VC and GUD modes, xlife, and others) that are still in wide use today. I thought
I knew how it was done.

Linux overturned much of what I thought I knew. I had been preaching the Unix gospel of small tools, rapid
prototyping and evolutionary programming for years. But I also believed there was a certain critical
complexity above which a more centralized, a priori approach was required. I believed that the most
important software (operating systems and really large tools like the Emacs programming editor) needed
to be built like cathedrals, carefully crafted by individual wizards or small bands of mages working in
splendid isolation, with no beta to be released before its time.

Linus Torvalds's style of developmentâ€”release early and often, delegate everything you can, be open to
the point of promiscuityâ€”came as a surprise. No quiet, reverent cathedral-building hereâ€”rather, the Linux
community seemed to resemble a great babbling bazaar of differing agendas and approaches (aptly
symbolized by the Linux archive sites, who'd take submissions from anyone) out of which a coherent and
stable system could seemingly emerge only by a succession of miracles.

The fact that this bazaar style seemed to work, and work well, came as a distinct shock. As I learned my
way around, I worked hard not just at individual projects, but also at trying to understand why the Linux
world not only didn't fly apart in confusion but seemed to go from strength to strength at a speed barely
imaginable to cathedral-builders.

By mid-1996 I thought I was beginning to understand. Chance handed me a perfect way to test my theory,
in the form of an open-source project that I could consciously try to run in the bazaar style. So I didâ€”and
it was a significant success.

This is the story of that project. I'll use it to propose some aphorisms about effective open-source
development. Not all of these are things I first learned in the Linux world, but we'll see how the Linux world
gives them particular point. If I'm correct, they'll help you understand exactly what it is that makes the Linux
community such a fountain of good softwareâ€”and, perhaps, they will help you become more productive
yourself.


## The Mail Must Get Through

Since 1993 I'd been running the technical side of a small free-access Internet service provider called Chester
County InterLink (CCIL) in West Chester, Pennsylvania. I co-founded CCIL and wrote our unique multiuser
bulletin-board softwareâ€”you can check it out by telnetting to locke.ccil.org. Today it supports almost three
thousand users on thirty lines. The job allowed me 24-hour-a-day access to the net through CCIL's 56K
lineâ€”in fact, the job practically demanded it!

I had gotten quite used to instant Internet email. I found having to periodically telnet over to locke to check
my mail annoying. What I wanted was for my mail to be delivered on snark (my home system) so that I
would be notified when it arrived and could handle it using all my local tools.

The Internet's native mail forwarding protocol, SMTP (Simple Mail Transfer Protocol), wouldn't suit, because
it works best when machines are connected full-time, while my personal machine isn't always on the Internet,
and doesn't have a static IP address. What I needed was a program that would reach out over my
intermittent dialup connection and pull across my mail to be delivered locally. I knew such things existed,
and that most of them used a simple application protocol called POP (Post Office Protocol). POP is now
widely supported by most common mail clients, but at the time, it wasn't built in to the mail reader I was
using.

I needed a POP3 client. So I went out on the Internet and found one. Actually, I found three or four. I used
one of them for a while, but it was missing what seemed an obvious feature, the ability to hack the addresses
on fetched mail so replies would work properly.

The problem was this: suppose someone named 'joe' on locke sent me mail. If I fetched the mail to snark
and then tried to reply to it, my mailer would cheerfully try to ship it to a nonexistent 'joe' on snark. Hand-
editing reply addresses to tack on <@ccil.org> quickly got to be a serious pain.

This was clearly something the computer ought to be doing for me. But none of the existing POP clients
knew how! And this brings us to the first lesson:

1. Every good work of software starts by scratching a developer's personal itch.

Perhaps this should have been obvious (it's long been proverbial that ''Necessity is the mother of invention'')
but too often software developers spend their days grinding away for pay at programs they neither need
nor love. But not in the Linux worldâ€”which may explain why the average quality of software originated in
the Linux community is so high.

So, did I immediately launch into a furious whirl of coding up a brand-new POP3 client to compete with
the existing ones? Not on your life! I looked carefully at the POP utilities I had in hand, asking myself
''Which one is closest to what I want?'' Because:


2. Good programmers know what to write. Great ones know what to rewrite (and reuse).

While I don't claim to be a great programmer, I try to imitate one. An important trait of the great ones is
constructive laziness. They know that you get an A not for effort but for results, and that it's almost always
easier to start from a good partial solution than from nothing at all.

Linus Torvalds, for example, didn't actually try to write Linux from scratch. Instead, he started by reusing
code and ideas from Minix, a tiny Unix-like operating system for PC clones. Eventually all the Minix code
went away or was completely rewrittenâ€”but while it was there, it provided scaffolding for the infant that
would eventually become Linux.

In the same spirit, I went looking for an existing POP utility that was reasonably well coded, to use as a
development base.

The source-sharing tradition of the Unix world has always been friendly to code reuse (this is why the GNU
project chose Unix as a base OS, in spite of serious reservations about the OS itself). The Linux world has
taken this tradition nearly to its technological limit; it has terabytes of open sources generally available. So
spending time looking for some else's almost-good-enough is more likely to give you good results in the
Linux world than anywhere else.

And it did for me. With those I'd found earlier, my second search made up a total of nine candidatesâ€”
fetchpop, PopTart, get-mail, gwpop, pimp, pop-perl, popc, popmail and upop. The one I first settled on was
'fetchpop' by Seung-Hong Oh. I put my header-rewrite feature in it, and made various other improvements
which the author accepted into his 1.9 release.

A few weeks later, though, I stumbled across the code for popclient by Carl Harris, and found I had a
problem. Though fetchpop had some good original ideas in it (such as its background-daemon mode), it
could only handle POP3 and was rather amateurishly coded (Seung-Hong was at that time a bright but
inexperienced programmer, and both traits showed). Carl's code was better, quite professional and solid,
but his program lacked several important and rather tricky-to -implement fetchpop features (including those
I'd coded myself).

Stay or switch? If I switched, I'd be throwing away the coding I'd already done in exchange for a better
development base.

A practical motive to switch was the presence of multiple-protocol support. POP3 is the most commonly
used of the post-office server protocols, but not the only one. Fetchpop and the other competition didn't
do POP2, RPOP, or APOP, and I was already having vague thoughts of perhaps adding IMAP (Internet
Message Access Protocol, the most recently designed and most powerful post-office protocol) just for fun.

But I had a more theoretical reason to think switching might be as good an idea as well, something I
learned long before Linux.


3. ''Plan to throw one away; you will, anyhow.'' (Fred Brooks, The Mythical Man-Month, Chapter 11)

Or, to put it another way, you often don't really understand the problem until after the first time you
implement a solution. The second time, maybe you know enough to do it right. So if you want to get it
right, be ready to start over at least once [JB].

Well (I told myself) the changes to fetchpop had been my first try. So I switched.

After I sent my first set of popclient patches to Carl Harris on 25 June 1996, I found out that he had basically
lost interest in popclient some time before. The code was a bit dusty, with minor bugs hanging out. I had
many changes to make, and we quickly agreed that the logical thing for me to do was take over the
program.

Without my actually noticing, the project had escalated. No longer was I just contemplating minor patches
to an existing POP client. I took on maintaining an entire one, and there were ideas bubbling in my head
that I knew would probably lead to major changes.

In a software culture that encourages code-sharing, this is a natural way for a project to evolve. I was acting
out this principle:

4. If you have the right attitude, interesting problems will find you.

But Carl Harris's attitude was even more important. He understood that

5. When you lose interest in a program, your last duty to it is to hand it off to a competent successor.

Without ever having to discuss it, Carl and I knew we had a common goal of having the best solution out
there. The only question for either of us was whether I could establish that I was a safe pair of hands. Once
I d id that, he acted with grace and dispatch. I hope I will do as well when it comes my turn.

## The Importance of Having Users

And so I inherited popclient. Just as importantly, I inherited popclient's user base. Users are wonderful things
to have, and not just because they demonstrate that you're serving a need, that you've done something
right. Properly cultivated, they can become co-developers.

Another strength of the Unix tradition, one that Linux pushes to a happy extreme, is that a lot of users are
hackers too. Because source code is available, they can be effective hackers. This can be tremendously useful
for shortening debugging time. Given a bit of encouragement, your users will diagnose problems, suggest
fixes, and help improve the code far more quickly than you could unaided.

6. Treating your users as co-developers is your least-hassle route to rapid code improvement and effective
debugging.

The power of this effect is easy to underestimate. In fact, pretty well all of us in the open-source world


drastically underestimated how well it would scale up with number of users and against system complexity,
until Linus Torvalds showed us differently.

In fact, I think Linus's cleverest and most consequential hack was not the construction of the Linux kernel
itself, but rather his invention of the Linux development model. When I expressed this opinion in his presence
once, he smiled and quietly repeated something he has often said: ''I'm basically a very lazy person who
likes to get credit for things other people actually do.'' Lazy like a fox. Or, as Robert Heinlein famously wrote
of one of his characters, too lazy to fail.

In retrospect, one precedent for the methods and success of Linux can be seen in the development of the
GNU Emacs Lisp library and Lisp code archives. In contrast to the cathedral-building style of the Emacs C
core and most other GNU tools, the evolution of the Lisp code pool was fluid and very user-driven. Ideas
and prototype modes were often rewritten three or four times before reaching a stable final form. And
loosely-coupled collaborations enabled by the Internet, a la Linux, were frequent.

Indeed, my own most successful single hack previous to fetchmail was probably Emacs VC (version control)
mode, a Linux-like collaboration by email with three other people, only one of whom (Richard Stallman, the
author of Emacs and founder of the Free Software Foundation) I have met to this day. It was a front-end
for SCCS, RCS and later CVS from within Emacs that offered ''one-touch'' version control operations. It
evolved from a tiny, crude sccs.el mode somebody else had written. And the development of VC succeeded
because, unlike Emacs itself, Emacs Lisp code could go through release/test/improve generations very
quickly.

The Emacs story is not unique. There have been other software products with a two-level architecture and
a two-tier user community that combined a cathedral-mode core and a bazaar-mode toolbox. One such is
MATLAB, a commercial data-analysis and visualization tool. Users of MATLAB and other products with a
similar structure invariably report that the action, the ferment, the innovation mostly takes place in the open
part of the tool where a large and varied community can tinker with it.

## Release Early, Release Often

Early and frequent releases are a critical part of the Linux development model. Most developers (including
me) used to believe this was bad policy for larger than trivial projects, because early versions are almost by
definition buggy versions and you don't want to wear out the patience of your users.

This belief reinforced the general commitment to a cathedral-building style of development. If the overriding
objective was for users to see as few bugs as possible, why then you'd only release a version every six
months (or less often), and work like a dog on debugging between releases. The Emacs C core was
developed this way. The Lisp library, in effect, was notâ€”because there were active Lisp archives outside the
FSF's control, where you could go to find new and development code versions independently of Emacs's
release cycle [QR].


The most important of these, the Ohio State Emacs Lisp archive, anticipated the spirit and many of the
features of today's big Linux archives. But few of us really thought very hard about what we were doing, or
about what the very existence of that archive suggested about problems in the FSF's cathedral-building
development model. I made one serious attempt around 1992 to get a lot of the Ohio code formally merged
into the official Emacs Lisp library. I ran into political trouble and was largely unsuccessful.

But by a year later, as Linux became widely visible, it was clear that something different and much healthier
was going on there. Linus's open development policy was the very opposite of cathedral-building. Linux's
Internet archives were burgeoning, multiple distributions were being floated. And all of this was driven by
an unheard-of frequency of core system releases.

Linus was treating his users as co-developers in the most effective possible way:

7. Release early. Release often. And listen to your customers.

Linus's innovation wasn't so much in doing quick-turnaround releases incorporating lots of user feedback
(something like this had been Unix-world tradition for a long time), but in scaling it up to a level of intensity
that matched the complexity of what he was developing. In those early times (around 1991) it wasn't
unknown for him to release a new kernel more than once a day! Because he cultivated his base of co-
developers and leveraged the Internet for collaboration harder than anyone else, this worked.

But how did it work? And was it something I could duplicate, or did it rely on some unique genius of Linus
To r va l d s?

I didn't think so. Granted, Linus is a damn fine hacker. How many of us could engineer an entire production-
quality operating system kernel from scratch? But Linux didn't represent any awesome conceptual leap
forward. Linus is not (or at least, not yet) an innovative genius of design in the way that, say, Richard
Stallman or James Gosling (of NeWS and Java) are. Rather, Linus seems to me to be a genius of engineering
and implementation, with a sixth sense for avoiding bugs and development dead-ends and a true knack
for finding the minimum-effort path from point A to point B. Indeed, the whole design of Linux breathes
this quality and mirrors Linus's essentially conservative and simplifying design approach.

So, if rapid releases and leveraging the Internet medium to the hilt were not accidents but integral parts of
Linus's engineering-genius insight into the minimum-effort path, what was he maximizing? What was he
cranking out of the machinery?

Put that way, the question answers itself. Linus was keeping his hacker/users constantly stimulated and
rewardedâ€”stimulated by the prospect of having an ego-satisfying piece of the action, rewarded by the
sight of constant (even daily) improvement in their work.

Linus was directly aiming to maximize the number of person-hours thrown at debugging and development,
even at the possible cost of instability in the code and user-base burnout if any serious bug proved


intractable. Linus was behaving as though he believed something like this:

8. Given a large enough beta-tester and co-developer base, almost every problem will be characterized
quickly and the fix obvious to someone.

Or, less formally, ''Given enough eyeballs, all bugs are shallow.'' I dub this: ''Linus's Law''.

My original formulation was that every problem ''will be transparent to somebody''. Linus demurred that
the person who understands and fixes the problem is not necessarily or even usually the person who first
characterizes it. ''Somebody finds the problem,'' he says, ''and somebody else understands it. And I'll go
on record as saying that finding it is the bigger challenge.'' That correction is important; we'll see how in
the next section, when we examine the practice of debugging in more detail. But the key point is that both
parts of the process (finding and fixing) tend to happen rapidly.

In Linus's Law, I think, lies the core difference underlying the cathedral-builder and bazaar styles. In the
cathedral-builder view of programming, bugs and development problems are tricky, insidious, deep
phenomena. It takes months of scrutiny by a dedicated few to develop confidence that you've winkled them
all out. Thus the long release intervals, and the inevitable disappointment when long-awaited releases are
not perfect.

In the bazaar view, on the other hand, you assume that bugs are generally shallow phenomenaâ€”or, at least,
that they turn shallow pretty quickly when exposed to a thousand eager co-developers pounding on every
single new release. Accordingly you release often in order to get more corrections, and as a beneficial side
effect you have less to lose if an occasional botch gets out the door.

And that's it. That's enough. If ''Linus's Law'' is false, then any system as complex as the Linux kernel, being
hacked over by as many hands as the that kernel was, should at some point have collapsed under the
weight of unforseen bad interactions and undiscovered ''deep'' bugs. If it's true, on the other hand, it is
sufficient to explain Linux's relative lack of bugginess and its continuous uptimes spanning months or even
years.

Maybe it shouldn't have been such a surprise, at that. Sociologists years ago discovered that the averaged
opinion of a mass of equally expert (or equally ignorant) observers is quite a bit more reliable a predictor
than the opinion of a single randomly-chosen one of the observers. They called this the Delphi effect. It
appears that what Linus has shown is that this applies even to debugging an operating systemâ€”that the
Delphi effect can tame development complexity even at the complexity level of an OS kernel. [CV]

One special feature of the Linux situation that clearly helps along the Delphi effect is the fact that the
contributors for any given project are self-selected. An early respondent pointed out that contributions are
received not from a random sample, but from people who are interested enough to use the software, learn
about how it works, attempt to find solutions to problems they encounter, and actually produce an
apparently reasonable fix. Anyone who passes all these filters is highly likely to have something useful to


contribute.

Linus's Law can be rephrased as ''Debugging is parallelizable''. Although debugging requires debuggers to
communicate with some coordinating developer, it doesn't require significant coordination between
debuggers. Thus it doesn't fall prey to the same quadratic complexity and management costs that make
adding developers problematic.

In practice, the theoretical loss of efficiency due to duplication of work by debuggers almost never seems
to be an issue in the Linux world. One effect of a ''release early and often'' policy is to minimize such
duplication by propagating fed-back fixes quickly [JH].

Brooks (the author of The Mythical Man-Month) even made an off-hand observation related to this: ''The
total cost of maintaining a widely used program is typically 40 percent or more of the cost of developing
it. Surprisingly this cost is strongly affected by the number of users. More users find more bugs.'' [emphasis
added].

More users find more bugs because adding more users adds more different ways of stressing the program.
This effect is amplified when the users are co-developers. Each one approaches the task of bug
characterization with a slightly different perceptual set and analytical toolkit, a different angle on the
problem. The ''Delphi effect'' seems to work precisely because of this variation. In the specific context of
debugging, the variation also tends to reduce duplication of effort.

So adding more beta-testers may not reduce the complexity of the current ''deepest'' bug from the
developer's point of view, but it increases the probability that someone's toolkit will be matched to the
problem in such a way that the bug is shallow to that person.

Linus coppers his bets, too. In case there are serious bugs, Linux kernel version are numbered in such a way
that potential users can make a choice either to run the last version designated ''stable'' or to ride the
cutting edge and risk bugs in order to get new features. This tactic is not yet systematically imitated by
most Linux hackers, but perhaps it should be; the fact that either choice is available makes both more
attractive. [HBS]

## How Many Eyeballs Tame Complexity

It's one thing to observe in the large that the bazaar style greatly accelerates debugging and code evolution.
It's another to understand exactly how and why it does so at the micro -level of day-to -day developer and
tester behavior. In this section (written three years after the original paper, using insights by developers who
read it and re-examined their own behavior) we'll take a hard look at the actual mechanisms. Non-technically
inclined readers can safely skip to the next section.

One key to understanding is to realize exactly why it is that the kind of bug report nonâ€“source-aware users
normally turn in tends not to be very useful. Nonâ€“source-aware users tend to report only surface symptoms;


they take their environment for granted, so they (a) omit critical background data, and (b) seldom include
a reliable recipe for reproducing the bug.

The underlying problem here is a mismatch between the tester's and the developer's mental models of the
program; the tester, on the outside looking in, and the developer on the inside looking out. In closed-
source development they're both stuck in these roles, and tend to talk past each other and find each other
deeply frustrating.

Open-source development breaks this bind, making it far easier for tester and developer to develop a
shared representation grounded in the actual source code and to communicate effectively about it.
Practically, there is a huge difference in leverage for the developer between the kind of bug report that just
reports externally-visible symptoms and the kind that hooks directly to the developer's source-codeâ€“based
mental representation of the program.

Most bugs, most of the time, are easily nailed given even an incomplete but suggestive characterization of
their error conditions at source-code level. When someone among your beta-testers can point out, "there's
a boundary problem in line nnn", or even just "under conditions X, Y, and Z, this variable rolls over", a quick
look at the offending code often suffices to pin down the exact mode of failure and generate a fix.

Thus, source-code awareness by both parties greatly enhances both good communication and the synergy
between what a beta-tester reports and what the core developer(s) know. In turn, this means that the core
developers' time tends to be well conserved, even with many collaborators.

Another characteristic of the open-source method that conserves developer time is the communication
structure of typical open-source projects. Above I used the term "core developer"; this reflects a distinction
between the project core (typically quite small; a single core developer is common, and one to three is
typical) and the project halo of beta-testers and available contributors (which often numbers in the
hundreds).

The fundamental problem that traditional software-development organization addresses is Brook's Law:
''Adding more programmers to a late project makes it later.'' More generally, Brooks's Law predicts that the
complexity and communication costs of a project rise with the square of the number of developers, while
work done only rises linearly.

Brooks's Law is founded on experience that bugs tend strongly to cluster at the interfaces between code
written by different people, and that communications/coordination overhead on a project tends to rise with
the number of interfaces between human beings. Thus, problems scale with the number of communications
paths between developers, which scales as the square of the humber of developers (more precisely,
according to the formula N*(N - 1)/2 where N is the number of developers).

The Brooks's Law analysis (and the resulting fear of large numbers in development groups) rests on a hidden
assummption: that the communications structure of the project is necessarily a complete graph, that


everybody talks to everybody else. But on open-source projects, the halo developers work on what are in
effect separable parallel subtasks and interact with each other very little; code changes and bug reports
stream through the core group, and only within that small core group do we pay the full Brooksian overhead.
[SU]

There are are still more reasons that source-codeâ€“level bug reporting tends to be very efficient. They center
around the fact that a single error can often have multiple possible symptoms, manifesting differently
depending on details of the user's usage pattern and environment. Such errors tend to be exactly the sort
of complex and subtle bugs (such as dynamic-memory-management errors or nondeterministic interrupt-
window artifacts) that are hardest to reproduce at will or to pin down by static analysis, and which do the
most to create long-term problems in software.

A tester who sends in a tentative source-codeâ€“level characterization of such a multi-symptom bug (e.g. "It
looks to me like there's a window in the signal handling near line 1250" or "Where are you zeroing that
bu ffer?") may give a developer, otherwise too close to the code to see it, the critical clue to a half-dozen
disparate symptoms. In cases like this, it may be hard or even impossible to know which externally-visible
misbehaviour was caused by precisely which bugâ€”but with frequent releases, it's unnecessary to know.
Other collaborators will be likely to find out quickly whether their bug has been fixed or not. In many cases,
source-level bug reports will cause misbehaviours to drop out without ever having been attributed to any
specific fix.

Complex multi-symptom errors also tend to have multiple trace paths from surface symptoms back to the
actual bug. Which of the trace paths a given developer or tester can chase may depend on subtleties of
that person's environment, and may well change in a not obviously deterministic way over time. In effect,
each developer and tester samples a semi-random set of the program's state space when looking for the
etiology of a symptom. The more subtle and complex the bug, the less likely that skill will be able to
guarantee the relevance of that sample.

For simple and easily reproducible bugs, then, the accent will be on the "semi" rather than the "random";
debugging skill and intimacy with the code and its architecture will matter a lot. But for complex bugs, the
accent will be on the "random". Under these circumstances many people running traces will be much more
effective than a few people running traces sequentiallyâ€”even if the few have a much higher average skill
level.

This effect will be greatly amplified if the difficulty of following trace paths from different surface symptoms
back to a bug varies significantly in a way that can't be predicted by looking at the symptoms. A single
developer sampling those paths sequentially will be as likely to pick a difficult trace path on the first try as
an easy one. On the other hand, suppose many people are trying trace paths in parallel while doing rapid
releases. Then it is likely one of them will find the easiest path immediately, and nail the bug in a much
shorter time. The project maintainer will see that, ship a new release, and the other people running traces


on the same bug will be able to stop before having spent too much time on their more difficult traces [RJ].

## When Is a Rose Not a Rose?

Having studied Linus's behavior and formed a theory about why it was successful, I made a conscious
decision to test this theory on my new (admittedly much less complex and ambitious) project.

But the first thing I did was reorganize and simplify popclient a lot. Carl Harris's implementation was very
sound, but exhibited a kind of unnecessary complexity common to many C programmers. He treated the
code as central and the data structures as support for the code. As a result, the code was beautiful but the
data structure design ad-hoc and rather ugly (at least by the high standards of this veteran LISP hacker).

I had another purpose for rewriting besides improving the code and the data structure design, however.
That was to evolve it into something I understood completely. It's no fun to be responsible for fixing bugs
in a program you don't understand.

For the first month or so, then, I was simply following out the implications of Carl's basic design. The first
serious change I made was to add IMAP support. I did this by reorganizing the protocol machines into a
generic driver and three method tables (for POP2, POP3, and IMAP). This and the previous changes illustrate
a general principle that's good for programmers to keep in mind, especially in languages like C that don't
naturally do dynamic typing:

9. Smart data structures and dumb code works a lot better than the other way around.

Brooks, Chapter 9: ''Show me your flowchart and conceal your tables, and I shall continue to be mystified.
Show me your tables, and I won't usually need your flowchart; it'll be obvious.'' Allowing for thirty years of
terminological/cultural shift, it's the same point.

At this point (early September 1996, about six weeks from zero) I started thinking that a name change
might be in orderâ€”after all, it wasn't just a POP client any more. But I hesitated, because there was as yet
nothing genuinely new in the design. My version of popclient had yet to develop an identity of its own.

That changed, radically, when popclient learned how to forward fetched mail to the SMTP port. I'll get to
that in a moment. But first: I said earlier that I'd decided to use this project to test my theory about what
Linus Torvalds had done right. How (you may well ask) did I do that? In these ways:

- I released early and often (almost never less often than every ten days; during periods of intense
    development, once a day).
- I grew my beta list by adding to it everyone who contacted me about fetchmail.
- I sent chatty announcements to the beta list whenever I released, encouraging people to participate.
- And I listened to my beta-testers, polling them about design decisions and stroking them whenever


'''
they sent in patches and feedback.
'''
The payoff from these simple measures was immediate. From the beginning of the project, I got bug reports
of a quality most developers would kill for, often with good fixes attached. I got thoughtful criticism, I got
fan mail, I got intelligent feature suggestions. Which leads to:

10. If you treat your beta-testers as if they're your most valuable resource, they will respond by becoming
your most valuable resource.

One interesting measure of fetchmail's success is the sheer size of the project beta list, fetchmail-friends. At
the time of latest revision of this paper (November 2000) it has 287 members and is adding two or three a
week.

Actually, when I revised in late May 1997 I found the list was beginning to lose members from its high of
close to 300 for an interesting reason. Several people have asked me to unsubscribe them because fetchmail
is working so well for them that they no longer need to see the list traffic! Perhaps this is part of the normal
life-cycle of a mature bazaar-style project.

## Popclient becomes Fetchmail

The real turning point in the project was when Harry Hochheiser sent me his scratch code for forwarding
mail to the client machine's SMTP port. I realized almost immediately that a reliable implementation of this
feature would make all the other mail delivery modes next to obsolete.

For many weeks I had been tweaking fetchmail rather incrementally while feeling like the interface design
was serviceable but grubbyâ€”inelegant and with too many exiguous options hanging out all over. The
options to dump fetched mail to a mailbox file or standard output particularly bothered me, but I couldn't
figure out why.

(If you don't care about the technicalia of Internet mail, the next two paragraphs can be safely skipped.)

What I saw when I thought about SMTP forwarding was that popclient had been trying to do too many
things. It had been designed to be both a mail transport agent (MTA) and a local delivery agent (MDA).
With SMTP forwarding, it could get out of the MDA business and be a pure MTA, handing off mail to other
programs for local delivery just as sendmail does.

Why mess with all the complexity of configuring a mail delivery agent or setting up lock-and-append on a
mailbox when port 25 is almost guaranteed to be there on any platform with TCP/IP support in the first
place? Especially when this means retrieved mail is guaranteed to look like normal sender-initiated SMTP
mail, which is really what we want anyway.

(Back to a higher level....)

Even if you didn't follow the preceding technical jargon, there are several important lessons here. First, this


SMTP-forwarding concept was the biggest single payoff I got from consciously trying to emulate Linus's
methods. A user gave me this terrific ideaâ€”all I had to do was understand the implications.

11. The next best thing to having good ideas is recognizing good ideas from your users. Sometimes the
latter is better.

Interestingly enough, you will quickly find that if you are completely and self-deprecatingly truthful about
how much you owe other people, the world at large will treat you as though you did every bit of the
invention yourself and are just being becomingly modest about your innate genius. We can all see how
well this worked for Linus!

(When I gave my talk at the first Perl Conference in August 1997, hacker extraordinaire Larry Wall was in
the front row. As I got to the last line above he called out, religious-revival style, ''Tell it, tell it, brother!''.
The whole audience laughed, because they knew this had worked for the inventor of Perl, too.)

After a very few weeks of running the project in the same spirit, I began to get similar praise not just from
my users but from other people to whom the word leaked out. I stashed away some of that email; I'll look
at it again sometime if I ever start wondering whether my life has been worthwhile :-).

But there are two more fundamental, non-political lessons here that are general to all kinds of design.

12. Often, the most striking and innovative solutions come from realizing that your concept of the problem
was wrong.

I had been trying to solve the wrong problem by continuing to develop popclient as a combined MTA/MDA
with all kinds of funky local delivery modes. Fetchmail's design needed to be rethought from the ground
up as a pure MTA, a part of the normal SMTP-speaking Internet mail path.

When you hit a wall in developmentâ€”when you find yourself hard put to think past the next patchâ€”it's
often time to ask not whether you've got the right answer, but whether you're asking the right question.
Perhaps the problem needs to be reframed.

Well, I had reframed my problem. Clearly, the right thing to do was (1) hack SMTP forwarding support into
the generic driver, (2) make it the default mode, and (3) eventually throw out all the other delivery modes,
especially the deliver-to -file and deliver-to -standard-output options.

I hesitated over step 3 for some time, fearing to upset long-time popclient users dependent on the alternate
delivery mechanisms. In theory, they could immediately switch to .forward files or their non-sendmail
equivalents to get the same effects. In practice the transition might have been messy.

But when I did it, the benefits proved huge. The cruftiest parts of the driver code vanished. Configuration
got radically simplerâ€”no more grovelling around for the system MDA and user's mailbox, no more worries
about whether the underlying OS supports file locking.


Also, the only way to lose mail vanished. If you specified delivery to a file and the disk got full, your mail
got lost. This can't happen with SMTP forwarding because your SMTP listener won't return OK unless the
message can be delivered or at least spooled for later delivery.

Also, performance improved (though not so you'd notice it in a single run). Another not insignificant benefit
of this change was that the manual page got a lot simpler.

Later, I had to bring delivery via a user-specified local MDA back in order to allow handling of some obscure
situations involving dynamic SLIP. But I found a much simpler way to do it.

The moral? Don't hesitate to throw away superannuated features when you can do it without loss of
effectiveness. Antoine de Saint-Exupï¿½ry (who was an aviator and aircraft designer when he wasn't authoring
classic children's books) said:

13. ''Perfection (in design) is achieved not when there is nothing more to add, but rather when there is
nothing more to take away.''

When your code is getting both better and simpler, that is when you know it's right. And in the process,
the fetchmail design acquired an identity of its own, different from the ancestral popclient.

It was time for the name change. The new design looked much more like a dual of sendmail than the old
popclient had; both are MTAs, but where sendmail pushes then delivers, the new popclient pulls then
delivers. So, two months off the blocks, I renamed it fetchmail.

There is a more general lesson in this story about how SMTP delivery came to fetchmail. It is not only
debugging that is parallelizable; development and (to a perhaps surprising extent) exploration of design
space is, too. When your development mode is rapidly iterative, development and enhancement may
become special cases of debuggingâ€”fixing 'bugs of omission' in the original capabilities or concept of the
software.

Even at a higher level of design, it can be very valuable to have lots of co-developers random-walking
through the design space near your product. Consider the way a puddle of water finds a drain, or better
yet how ants find food: exploration essentially by diffusion, followed by exploitation mediated by a scalable
communication mechanism. This works very well; as with Harry Hochheiser and me, one of your outriders
may well find a huge win nearby that you were just a little too close-focused to see.

## Fetchmail Grows Up

There I was with a neat and innovative design, code that I knew worked well because I used it every day,
and a burgeoning beta list. It gradually dawned on me that I was no longer engaged in a trivial personal
hack that might happen to be useful to few other people. I had my hands on a program that every hacker
with a Unix box and a SLIP/PPP mail connection really needs.


SMTP Æ÷¿öµù Æ¯Â¡°ú ¾î¿ï·¯, ÀÌ°ÍÀº ÀáÀçÀûÀ¸·Î "Ä«Å×°í¸® Å³·¯"°¡ µÉ ¸¸Å­ °æÀï ¾ÕÀ¸·Î ²ø·Á ³ª¿Ô´Ù, "Ä«Å×°í¸® Å³·¯"¶õ ´Ù¸¥ °ÍµéÀÌ ¹«½Ã´çÇÏ´Â °ÍÀÌ ¾Æ´Ï¶ó °ÅÀÇ ÀØÇôÁú Á¤µµ·Î ÀûÇÕÇÑ Àå¼Ò¿¡ ÀûÀıÇÏ°Ô Ã¤¿î ÀüÇüÀûÀÎ ÇÁ·Î±×·¥ Áß ÇÏ³ªÀÌ´Ù.

³ª´Â ´ç½ÅÀÌ ÀÌ°Í°ú °°Àº °á°ú¸¦ À§ÇØ ¿­¸ÁÇÏ°Å³ª °èÈ¹ÇÒ ¼ö ÀÖ´Ù°í »ı°¢ÇÏÁö´Â ¾Ê´Â´Ù. ´ç½ÅÀº ³ªÁß¿¡ °á°ú°¡ ÇÊ¿¬ÀûÀÌ°í, ÀÚ¿¬½º·´°í, ¿ÀÈ÷·Á ¹Ì¸® Á¤ÇØÁø °ÍÃ³·³ º¸ÀÏ¸¸Å­ °­·ÂÇÑ ¼³°è ¾ÆÀÌµğ¾î·Î ±×°Í¿¡ ºüÁ®µé¾î¾ß ÇÑ´Ù. ÀÌ¿Í °°Àº ¾ÆÀÌµğ¾î¸¦ À§ÇØ ³ë·ÂÇÒ ¼ö ÀÖ´Â ¹æ¹ıÀº ¿ÀÁ÷ ¼ö¸¹Àº ¾ÆÀÌµğ¾î¸¦ °¡Áö°í ÀÖ°Å³ª, ´Ù¸¥ »ç¶÷µéÀÇ ÁÁÀº ¾ÆÀÌµğ¾îµéÀ» Ã¢ÀÛÀÚ°¡ °¡¾ßÇÑ´Ù°í »ı°¢ÇÏ´Â ¹æÇâÀ¸·Î ÀÌ²ø ¸¸Å­ÀÇ °øÇĞÀû Á¤ÀÇ¸¦ °¡Áö°í ÀÖ´Â °Í ¹Û¿¡ ¾ø´Ù.

¾Øµğ Å¸³Ù¹Ù¿òÀº °¡¸£Ä¡´Â ¿ëµµ(±×´Â ÀÌ°ÍÀ» ¹Ì´Ğ½ºMinix·Î ºÒ·¶´Ù)·Î ¾²±âÀ§ÇØ, IBM PC Àü¿ë °£´ÜÇÑ À¯´Ğ½º(Unix)¸¦ ¼³°èÇÒ ¿ø·¡ÀÇ »ı°¢À» °¡Áö°í ÀÖ¾ú´Ù. ¸®´ª½º Åä¹ßÁî´Â ±× ¹Ì´Ğ½º °³³äÀ» ¾Øµå·ù°¡ ¾Æ¸¶µµ ±×°ÍÀÌ °¡¾ßÇÏ´Â ¹æÇâÀÌ¶ó°í »ı°¢ÇÑ °Íº¸´Ù ÈÎ¾À ´õ ¹ßÀü½ÃÄ×´Ù, ±×¸®°í ±×°ÍÀº ¾ÆÁÖ ¸ÚÁø °ÍÀ¸·Î ¼ºÀåÇß´Ù. °°Àº ¹æ½ÄÀ¸·Î (´õ ÀÛÀº ±Ô¸ğ·Î), ³ª´Â Ä® ÇØ¸®½º¿Í ÇØ¸® È£Å©ÇìÀÌ¼­·ÎºÎÅÍ ¸î°¡Áö ¾ÆÀÌµğ¾î¸¦ ¾ò¾ú°í ±×µéÀ» ¿­½ÉÈ÷ ¹ßÀü½ÃÄ×´Ù. ¿ì¸® Áß ¾Æ¹«µµ »ç¶÷µéÀÌ ÃµÀçÀûÀÌ¶ó°í »ı°¢ÇÏ´Â ³¶¸¸ÀûÀÎ ¹æ½ÄÀ¸·Î 'µ¶Ã¢Àû'ÀÌÁö ¸øÇß´Ù. ±×¶õ, ´ëºÎºĞÀÇ °úÇĞ°ú °øÇĞ, ±×¸®°í ¼ÒÇÁÆ®¿ş¾î »ê¾÷Àº ¿ø·¡ ÃµÀçÀÎ ÇØÄ¿ÀÇ ½ÅÈ­¿¡ ÀÇÇØ ¹İ´ë·Î ÀÌ·ç¾îÁöÁö ¾Ê´Â´Ù.

±× °á°ú´Â ±×·³¿¡µµ ºÒ±¸ÇÏ°í ²Ï µé¶ß°Ô ÇÑ´Ù-»ç½Ç, ±×·¯ÇÑ ¼º°øÀº ¸ğµç ÇìÄ¿µéÀÌ °¥¸ÁÇÏ´Â °ÍÀÌ´Ù! ±×¸®°í ±×µéÀº ³»°¡ ³» ±âÁØÀ» Á» ´õ ³ô°Ô Àâ¾Æ¾ß ÇÏ´Â °ÍÀ» ÀÇ¹ÌÇÑ´Ù. ÇöÀç °¡´ÉÇÒ ¸¸Å­ ÀÎÃâ ¸ŞÀÏÀ» ÁÁ°Ô ¸¸µé±â À§ÇØ¼­´Â, ³ª´Â ³» ÇÊ¿ä¸¸À» À§ÇØ ¾µ »Ó¸¸ ¾Æ´Ï¶ó, ³» ¿µ¿ªÀº ¾Æ´ÏÁö¸¸ ´Ù¸¥ »ç¶÷µé¿¡°Ô ÇÊ¿äÇÑ ±â´ÉµéÀ» Áö¿øÇÏ°í Æ÷ÇÔ½ÃÄÑ¾ß ÇÑ´Ù. ±×¸®°í ÇÁ·Î±×·¥À» °£´ÜÇÏ°í °ß°íÇÏ°Ô²û ÇÏ´Â ¿ÍÁß¿¡ ±× ÀÛ¾÷À» ÇÏ´Â °ÍÀÌ´Ù.

³»°¡ ÀÌ¸¦ ±ú´İ°í ³­ ÈÄ¿¡ ¾²´Â °¡Àå Ã¹¹øÂ°ÀÌ°í ¾ĞµµÀûÀ¸·Î Áß¿äÇÑ ±â´ÉÀº ¸ÖÆ¼µå·Ó ¼­Æ÷Æ®ÀÌ´Ù(multidrop support)-ÀÌ°ÍÀº »ç¿ëÀÚ ±×·ìÀÇ ¸ğµç ¸ŞÀÏÀ» ÃàÃ´ÇÏ°í ÀÖ´Â ¸ŞÀÏ¹Ú½º·ÎºÎÅÍ ¸ŞÀÏÀ» Àâ¾Æ¿À°í, °¢°¢ÀÇ ¸ŞÀÏÀ» ÇØ´çÇÏ´Â ¼ö½ÅÀÚ¿¡°Ô Àü¼ÛÇÏ´Â ´É·ÂÀ» ÀÇ¹ÌÇÑ´Ù.

³ª´Â ¸ÖÆ¼µå·Ó ¼­Æ÷Æ® ±â´ÉÀ» Ãß°¡ÇÏ±â·Î ´ÙÁüÇß´Ù, ¿Ö³ÄÇÏ¸é ¸î¸î »ç¿ëÀÚµéÀº ÀÌ ±â´ÉÀ» ¿ä±¸ÇÏ±â ¶§¹®ÀÌ´Ù, ±×·¯³ª ÁÖµÈ ÀÌÀ¯´Â ÀÌ ±â´ÉÀÌ ³ª·Î ÇÏ¿©±İ ¸ğµç °æ¿ì¿¡ ´ëÇÏ¿© ÁÖ¼Ò ÁöÁ¤À» ´Ù·çµµ·Ï ÇÏ¿©, ½Ì±Û µå·ÓÀ¸·ÎºÎÅÍ ¹ö±×¸¦ ÇØ°áÇÒ ¼ö ÀÖ´Ù°í »ı°¢ÇÏ±â ¶§¹®ÀÌ´Ù. ±×¸®°í ÀÌ´Â Áõ¸íÀÌ µÇ¾ú´Ù. RFC 822ÀÇ ÁÖ¼Ò ÆÄ½ÌÀ» ¾Ë¸Â°Ô ÇÏ´Â °ÍÀº ¾Æ¹« ¾öÃ»³­ ½Ã°£ÀÌ °É·È´Ù, ±×°ÍÀÇ °¢ ºÎºĞÀÌ ¾î·Á¿î °ÍÀÌ ¾Æ´Ï¶ó, »ê´õ¹ÌÀÇ »óÈ£ µ¶¸³ÀûÀÌ°í ±î´Ù·Î¿î ¼¼ºÎ»çÇ×µéÀ» Æ÷ÇÔÇÏ°í ÀÖ±â ¶§¹®ÀÌ´Ù.

±×·¯³ª ¸ÖÆ¼µå·Ó ÁÖ¼ÒÁöÁ¤Àº ¾ÆÁÖ ¶Ù¾î³­ ¼³°è ¹æ¾ÈÀ¸·Î ¹àÇôÁ³´Ù. ÀÌ´Â ³»°¡ ¾î¶»°Ô ¾Ë¾Ò´ÂÁö¿¡ ´ëÇÑ ³»¿ëÀÌ´Ù:


14. ¾î¶°ÇÑ µµ±¸µµ ¿¹Ãø°¡´ÉÇÑ ¹æ½ÄÀ¸·Î À¯¿ëÇØ¾ß ÇÑ´Ù, ±×·¯³ª ½ÇÁ¦ ÈÇ¸¢ÇÑ µµ±¸´Â ´ç½ÅÀÌ Àı´ë·Î ¿¹Ãø ÇÒ ¼ö ¾ø´Â ¹æ½ÄÀ¸·Î »ç¿ëÇÏ°Ô²û ½º½º·Î¸¦ Á¦°øÇÑ´Ù.

¸ÖÆ¼µå·Ó ÀÎÃâ¸ŞÀÏÀÇ ¿¹ÃøºÒ°¡ÇÑ ¹æ½ÄÀº ¿ìÆí¹° ¼ö½ÅÀÚ ¸í´ÜÀ» º¸È£ÇÑÃ¤·Î ½ÇÇà½ÃÅ°°í ÀÎÅÍ³İ ¿¬°á»ó¿¡¼­ Å¬¶óÀÌ¾ğÆ®(client) Ãø¿¡¼­ °¡¸í È®ÀåÀ» ¸¶Ä¡´Â °ÍÀÌ´Ù. ÀÌ´Â ´©±º°¡°¡ ISP °èÁÂ¸¦ ÅëÇØ °³ÀÎÀûÀÎ ±â°è¸¦ ½ÇÇà½ÃÅ³ ¼ö ÀÖÀ¸¸ç ISPÀÇ °¡¸í ÆÄÀÏ¿¡ ´ëÇÑ Áö¼ÓÀûÀÎ Á¢±Ù ¾øÀÌµµ ¼ö½ÅÀÚ ¸í´ÜÀ» °ü¸®ÇÒ ¼ö ÀÖÀ½À» ÀÇ¹ÌÇÑ´Ù.

³ªÀÇ º£Å¸ Å×½ºÅÍ¿¡ ÀÇÇØ ¿ä±¸µÈ ¶Ç´Ù¸¥ Áß¿äÇÑ º¯È­´Â 8ºñÆ® Â¥¸®ÀÇ MIME ¿¬»êÀ» Áö¿øÇÏ´Â °ÍÀÌ¾ú´Ù. ÀÌ´Â ²Ï ½ÇÇàÇÏ±â ½¬¿ü´Âµ¥, ¿Ö³ÄÇÏ¸é ³ª´Â ÇØ´ç ÄÚµå°¡ 8ºñÆ® ±ú²ıÇÏµµ·Ï ÇØ¿Ô±â ¶§¹®ÀÌ´Ù.(Áï, 8ºñÆ®¸¦ ¾ĞÃàÇÏ´Â °ÍÀÌ ¾Æ´Ï¶ó, ¾Æ½ºÅ° ¹®ÀÚ ÁıÇÕ¿¡¼­ »ç¿ëÇÏÁö ¾Ê´Â ºÎºĞÀ» ÇÁ·Î±×·¥ ³»¿¡¼­ Á¤º¸¸¦ °¡Á®¿À±â À§ÇØ ¼­ºñ½º ¾ÈÀ¸·Î ³Ö´Â °ÍÀÌ´Ù) ³»°¡ ÀÌ ±â´É¿¡ ´ëÇÑ ¿ä±¸¸¦ ¿¹ÃøÇß±â ¶§¹®ÀÌ ¾Æ´Ï¶ó, ¶Ç´Ù¸¥ ±ÔÄ¢¿¡ µû¸¥ °ÍÀÌ´Ù:


15. ¾î¶² Á¾·ùÀÇ °ÔÀÌÆ®¿şÀÌ ±â¹İ ¼ÒÇÁÆ®¿ş¾î¸¦ ¼³°èÇÒ ¶§´Â, µ¥ÀÌÅÍ ½ºÆ®¸²À» ÃÖ´ëÇÑ ¹æÇØÇÏÁö ¾Êµµ·Ï ³ë·ÂÇÏ´Â °ÍÀÌ´Ù-±×¸®°í °í°´ÀÌ ´ç½ÅÀ¸·Î ÇÏ¿©±İ °­Á¦ÇÏÁö ¾Ê´Â´Ù¸é Á¤º¸¸¦ Àı´ë ¹ö¸®Áö ¸»¾Æ¶ó!

¸¸¾à ³»°¡ ÀÌ ±ÔÄ¢À» µû¸£Áö ¾Ê¾Ò´Ù¸é, 8ºñÆ® Â¥¸®ÀÇ MIME Áö¿øÀº ¾î·Æ°Å³ª ¹ö±×°¡ ¸¹ÀÌ ³µÀ» ¼öµµ ÀÖ´Ù. »ç½ÇÀº ±×·¸Áö ¾Ê¾ÒÀ¸¹Ç·Î, ³»°¡ ÇØ¾ß Çß´ø °ÍÀº ¿ÀÁ÷ MIME Ç¥ÁØ(RFC 1625)À» ÀĞ°í Çì´õ-»ı¼º ·ÎÁ÷¿¡ ÀÛÀº ºñÆ®¸¦ Ãß°¡ÇÏ´Â °Í »ÓÀÌ¾ú´Ù.

¸î¸î À¯·´ ÀÌ¿ëÀÚµéÀº (±×µéÀÌ ±×µéÀÇ ºñ½Ñ ÇÚµåÆù ³×Æ®¿öÅ©·Î ÀÎÇÑ ºñ¿ëÀ» Á¶ÀıÇÏ±â À§ÇØ) ¼¼¼Ç¸¶´Ù È¸¼öÇÏ´Â ¸Ş¼¼ÁöÀÇ °³¼ö¸¦ Á¦ÇÑÇÏ´Â ¿É¼ÇÀ» Ãß°¡ÇÏ¶ó°í ³ª¸¦ ±«·ÓÈù´Ù. ³ª´Â ÀÌ¸¦ ¿À·§µ¿¾È ÀúÇ×ÇØ¿Ô°í, ±×¸®°í ³ª´Â ¾ÆÁ÷ ÀÌºÎºĞ¿¡ ´ëÇØ ¿ÏÀüÈ÷ ±âºĞ ÁÁÁö ¾Ê´Ù. ±×·¯³ª ¸¸¾à ´ç½ÅÀÌ ¼¼°è¸¦ À§ÇØ ½áÁØ´Ù¸é, ´ç½ÅÀº ´ç½ÅÀÇ °í°´µéÀÇ ¸»À» µé¾î¾ß ÇÒ °ÍÀÌ´Ù-ÀÌ´Â ±×µéÀÌ ´ÜÁö ´ç½Å¿¡°Ô µ·À» ÁöºÒÇÑ´Ù°í ÇØ¼­ ¹Ù²îÁö ¾Ê´Â´Ù.


## ÀÎÃâ ¸ŞÀÏ(fetchmail)·ÎºÎÅÍ ¸î°¡Áö ¹è¿ï Á¡µé

¼ÒÇÁÆ®¿ş¾î ¿£Áö´Ï¾î¿¡ °ü·ÃµÈ ¸î°¡Áö Áß¿ä»çÇ×µé·Î °¡±âÀü¿¡, ¼÷°íÇÒ ¸¸ÇÑ ÀÎÃâ °æÇèÀ¸·Î ºÎÅÍÀÇ Æ¯Á¤ ¹è¿ï Á¡µéÀÌ ´õ ÀÖ´Ù. ºñÀü¹®ÀûÀÎ µ¶ÀÚµéÀº ÀÌ ºÎºĞÀ» »ı·«ÇØµµ ÁÁ´Ù.

rc (ÄÁÆ®·Ñ) ÆÄÀÏ ¹®¹ıÀº ÆÄ¼­¿¡ ÀÇÇØ¼­ ¿ÏÀüÈ÷ ¹«½ÃµÇ´Â ¼±ÅÃÀûÀÎ 'ÀâÀ½' Å°¿öµå¸¦ Æ÷ÇÔÇÏ°í ÀÖ´Ù. ±×µéÀÌ ÁØ¼öÇÏ´Â ¿µ¾î¿Í ºñ½ÁÇÏ°Ô »ı±ä ¹®¹ıÀº ´ç½ÅÀÌ ¿ÏÀüÈ÷ ±×°ÍµéÀ» Á¦°ÅÇßÀ» ¶§ °®´Â ÀüÅëÀûÀÎ °£´ÜÇÑ Å°¿öµå-°ª Â¦º¸´Ù´Â ÈÎ¾À °¡µ¶¼ºÀÌ ÀÖ´Ù.

ÀÌ·± °ÍµéÀº ³»°¡ ¾ó¸¶³ª rc ÆÄÀÏ ¼±¾ğµéÀÌ ±ä¿äÇÑ ¼Ò¾ğ¾î(minilanguage)¸¦ ´à¾Æ °¡±â ½ÃÀÛÇÏ´ÂÁö¸¦ ±ú´Ş¾ÒÀ» ¶§ÀÇ Áö³­ ¹ã °æÇèÀ¸·Î ºÎÅÍ ½ÃÀÛÇÑ´Ù. (ÀÌ°ÍÀº ¶ÇÇÑ ³»°¡ ¿ø·¡ ÆËÅ¬¶óÀÌ¾ğÆ®(popclient) "server" Å°¿öµå¸¦ "poll"·Î ¹Ù²Û ÀÌÀ¯ÀÌ±âµµ ÇÏ´Ù)

±×·¯ÇÑ ±ä¿äÇÑ ¼Ò¾ğ¾î¸¦ ¿µ¾îÃ³·³ ¸¸µå´Â °ÍÀº ³ª¿¡°Ô ¼Ò¾ğ¾î¸¦ ´õ ½±°Ô ¾²°Ô ÇØÁÖ´Â °ÍÃ³·³ º¸¿´´Ù. ÇöÀç, ³»°¡ Emacs³ª HTML ±×¸®°í ¸¹Àº µ¥ÀÌÅÍº£ÀÌ½º ¿£Áøµé¿¡ ÀÇÇØ ÀüÇüÀûÀÎ ¿¹°¡ µÇ°í ÀÖ´Â ¼³°èÀÇ "¾ğ¾î·Î ¸¸µé±â" ÆÄÀÇ ¿­·ÄÇÑ ÁöÁöÀÚÀÓ¿¡µµ ºÒ±¸ÇÏ°í, ³ª´Â "¿µ¾î¿Í ºñ½ÁÇÑ" ¹®¹ıÀÇ Å« ÁöÁöÀÚ´Â ¾Æ´Ï´Ù.

ÀüÅëÀûÀ¸·Î ÇÁ·Î±×·¡¸ÓµéÀº ¸Å¿ì Á¤È®ÇÏ°í ²Ä²ÄÇÏ°í, ±×¸®°í ºÒÇÊ¿äÇÑ Áßº¹ÀÌ ¾ø´Â Á¦¾î ¹®¹ıµéÀ» ¼±È£ÇÏ´Â °æÇâÀÌ ÀÖ´Ù. ÀÌ´Â ÄÄÇ»ÅÍ ÀÚ¿øµéÀÌ ¸Å¿ì ºñ½Õ±â¶§¹®¿¡ ³ª¿À´Â ¹®È­ÀûÀÎ À¯»êÀÌ¸ç, µû¶ó¼­ ´Ü°è¸¦ ÆÄ½ÌÇÏ´Â °ÍÀº °¡´ÉÇÑÇÑ ¸Å¿ì Àú·ÅÇÏ°í °£´ÜÇØ¾ß ÇÑ´Ù. 50%ÀÇ ºÒÇÊ¿äÇÑ Áßº¹À» °¡Áö°í ÀÖ´Â ¿µ¾î´Â ¾ÆÁÖ ÀûÀıÇÏÁö ¾ÊÀº ¸ğµ¨Ã³·³ º¸ÀÎ´Ù.

ÀÌ´Â ¿µ¾î¿Í ºñ½ÁÇÑ ¹®¹ıÀ» ÇÇÇÏ´Â ³ªÀÇ ÀÌÀ¯´Â ¾Æ´Ï´Ù; ³ª´Â ÀÌ¸¦ µÚÁı±â À§ÇØ ¾ğ±ŞÇÏ¿´´Ù. ¸Å¿ì Àú·ÅÇÑ È¸·Î¿Í ÄÚ¾î¸¦ °¡Áö°í, °£°áÇÔÀº ±×°ÍÀ¸·Î ³¡³ª¸é ¾ÈµÈ´Ù. ¿À´Ã³¯ ¾ğ¾î°¡ ÄÄÇ»ÅÍ¸¦ À§ÇØ Àú·ÅÇÑ °Íº¸´Ù´Â ÀÎ°£µéÀ» À§ÇØ Æí¸®ÇÑ °ÍÀÌ ´õ Áß¿äÇÏ´Ù.

±×·¯³ª, Á¶½ÉÇØ¾ßÇÏ´Â ÁÁÀº ÀÌÀ¯µéÀÌ ¿©ÀüÈ÷ ³²¾ÆÀÖ´Ù. ÇÏ³ª´Â ÆÄ½Ì ´Ü°èÀÇ º¹ÀâÇÑ ºñ¿ëÀÌ´Ù-´ç½ÅÀº ºñ¿ëÀÌ ¹ö±×ÀÇ Áß¿äÇÑ ¿øÀÎÀÌ µÇ°Å³ª ±×°Í ÀÚÃ¼·Î »ç¿ëÀÚÀÇ È¥¶õÀ» ºÒ·¯ÀÏÀ¸Å°´Â ÁöÁ¡À¸·Î ±îÁö Áõ°¡ÇÏÁö ¾Ê±â¸¦ ¿øÇÑ´Ù. ¶Ç´Ù¸¥ ÀÌÀ¯´Â ¾ğ¾î¹®¹ıÀ» ¿µ¾îÃ³·³ ¸¸µå´Â °ÍÀº Á¾Á¾ "¿µ¾î" ±× ÀÚÃ¼°¡ ½É°¢ÇÏ°Ô ÇüÅÂ¸¦ ¹ş¾î³ª±â ¶§¹®ÀÌ´Ù, µû¶ó¼­ ÀÚ¿¬¾î¿¡ ÇÇ»óÀûÀ¸·Î ´àÀº °ÍÀº ÀüÅëÀûÀÎ ¹®¹ıÀÌ °¡Á®¿Ô´ø °ÍÃ³·³ È¥¶õ½º·´´Ù. (´ç½ÅÀº ÀÌ·± ¾Ç¿µÇâÀ» ¼ÒÀ§ ¸»ÇÏ´Â "4¼¼´ë"¿Í »ó¾÷ÀûÀÎ µ¥ÀÌÅÍº£ÀÌ½º ÁúÀÇ¾î¿¡¼­ º¸¾Æ¿Ô´Ù)

ÀÎÃâ ¸ŞÀÏ Á¦¾î ¹®¹ıÀº ÀÌ·¯ÇÑ ¹®Á¦µéÀ» ÇÇÇÏ´Â °ÍÃ³·³ º¸ÀÎ´Ù, ¿Ö³ÄÇÏ¸é ÀÌ ¾ğ¾î ¿µ¿ªÀº ¸Å¿ì Á¦ÇÑµÇ¾î ÀÖ±â ¶§¹®ÀÌ´Ù. ÀÌ´Â ¹ü¿ë ¸ñÀûÀÇ ¾ğ¾î ±ÙÃ³¿¡ ¾îµğ¿¡µµ ¾ø´Ù; ÀÌ°ÍÀÌ °£´ÜÈ÷ ¸»ÇÏ´Â ¹Ù´Â ±×·¸°Ô º¹ÀâÇÏÁö´Â ¾Ê´Ù, µû¶ó¼­ ¿µ¾îÀÇ ¸Å¿ì ÀÛÀº ÀÏºÎ¿Í ½ÇÁ¦ Á¦¾î ¾ğ¾î »çÀÌ¿¡ Á¤½ÅÀûÀ¸·Î ÀÌµ¿ÇÏ´Â °ÍÀÇ È¥¶õÀº ¸Å¿ì ÀûÀº °¡´É¼ºÀÌ ÀÖ´Ù. ³ª´Â ÀÌ·± °Í¿¡ ´õ ³Ğ°Ô ¹è¿ï Á¡ÀÌ ÀÖ´Ù°í »ı°¢ÇÑ´Ù;


16. ´ç½ÅÀÇ ¾ğ¾î°¡ Turing-complete °¡±îÀÌ¿¡ ¾ø´Ù¸é, ¹®¹ıÀû ¼³ÅÁÀº ´ç½ÅÀÇ Ä£±¸°¡ µÉ °ÍÀÌ´Ù.

¶Ç´Ù¸¥ ¹è¿ï Á¡Àº ¸ğÈ£ÇÔ¿¡ ÀÇÇÑ º¸¾È¿¡ °üÇÑ °ÍÀÌ´Ù. ¸î¸î ÀÎÃâ ¸ŞÀÏ »ç¿ëÀÚµéÀº ³ª¿¡°Ô rcÆÄÀÏ¿¡¼­ ºñ¹Ğ¹øÈ£¸¦ ¾ÏÈ£È­ÇÑÃ¤ ÀúÀå ÇÔÀ¸·Î¼­ ½ºÆÄÀÌ°¡ ±×µéÀ» º¸Áö ¾Êµµ·Ï ¼ÒÇÁÆ®¿ş¾î¸¦ ¹Ù²ã ´Ş¶ó°í ¿ä±¸ÇÑ´Ù.

³ª´Â ÀÌ°ÍÀ» ¹Ù²ÙÁö ¾Ê´Â´Ù, ¿Ö³ÄÇÏ¸é ÀÌ°ÍÀÌ »ç½Ç»ó º¸È£¸¦ Ãß°¡ÇÏÁö ¾Ê±â ¶§¹®ÀÌ´Ù. ´ç½ÅÀÇ rc ÆÄÀÏÀ» ÀĞµµ·Ï Çã°¡¸¦ ¹ŞÀº »ç¶÷ ´©±¸³ª ¾îÂ·µç ´ç½ÅÃ³·³ ÀÎÃâ ¸ŞÀÏÀ» °ü¸®ÇÒ ¼ö ÀÖ´Ù-±×¸®°í ±×µéÀÌ Ã£´Â °ÍÀÌ ´ç½ÅÀÇ ºñ¹Ğ¹øÈ£¶ó¸é, ±×µéÀº ÇÊ¿äÇÑ ÇØµ¶±â(decoder)¸¦ °®±â À§ÇØ ÀÌ¸¦ ÀÎÃâ ¸ŞÀÏ ÄÚµå¿Í ¶¼¾î³¾ ¼ö ÀÖ´Ù.

ÀÎÃâ ¸ŞÀÏ ºñ¹Ğ¹øÈ£ ¾ÏÈ£È­°¡ Çß´ø ¸ğµç °ÍÀº Àß »ı°¢ÇÏÁö ¾Ê´Â »ç¶÷µé¿¡°Ô º¸¾È¿¡ ´ëÇÑ Àß¸øµÈ ÀÎ½ÄÀ» ÁÖ´Â °ÍÀÌ´Ù. ¹ü¿ëÀûÀÎ ±ÔÄ¢Àº ¿©±â ÀÖ´Ù;


17. º¸¾È ½Ã½ºÅÛÀº ±×°ÍÀÇ ºñ¹Ğ¸¸Å­ÀÌ³ª ¾ÈÀüÇÏ´Ù. °¡Â¥ ºñ¹ĞÀ» Á¶½ÉÇÏ¶ó.

## ½ÃÀå(Bazaar) ½ºÅ¸ÀÏÀ» À§ÇÑ ÇÊ¿äÇÑ ÀüÁ¦Á¶°Ç

ÀÌ·± ³í¹®ÀÇ ÃÊ±â °Ë½ÃÀÚµé°ú ½ÃÇè µ¶ÀÚµéÀº Áö¼ÓÀûÀ¸·Î, ÇÏ³ª°¡ °ø°øÀûÀÌ°Ô µÇ°í °øµ¿ °³¹ßÀÚ Ä¿¹Â´ÏÆ¼¸¦ »ı¼ºÇÏ±â ½ÃÀÛÇÏ´Â ±×¶§¿¡ ÇÁ·ÎÁ§Æ® ¸®´õÀÇ ÀÚ°İ°ú ÄÚµåÀÇ »óÅÂ ¸ğµÎ¸¦ Æ÷ÇÔÇÑ ¼º°øÀûÀÎ ½ÃÀå-½ºÅ¸ÀÏ °³¹ß¿¡ ´ëÇÑ ÀüÁ¦Á¶°Çµé¿¡ °ü·ÃµÈ Áú¹®À» Á¦±âÇß´Ù.

½ÃÀå ½ºÅ¸ÀÏ¿¡¼­ °³ÀÎ ÇÑ¸íÀÌ ÄÚµå¸¦ Ã³À½ºÎÅÍ ÀÛ¼ºÇÏÁö ¸øÇÏ´Â °ÍÀº ²Ï ºĞ¸íÇÏ´Ù. ¾î¶² ÇÑ¸íÀº ½ÃÇè°ú µğ¹ö±×, ±×¸®°í ½ÃÀå ½ºÅ¸ÀÏ·Î ¹ßÀü½ÃÅ³ ¼ö ÀÖ´Ù, ±×·¯³ª ½ÃÀå ¸ğµå·Î ÇÁ·ÎÁ§Æ®¸¦ »ı¼ºÇÏ´Â °ÍÀº ¸Å¿ì ¾î·Æ´Ù. ¸®´ª½º´Â ÀÌ¸¦ ½ÃµµÇÏÁö ¾Ê¾Ò´Ù. ³ªµµ ¸¶Âù°¡ÁöÀÌ´Ù. ´ç½ÅÀÇ ÃÊ±â °³¹ßÀÚ Ä¿¹Â´ÏÆ¼´Â ½ÇÇàÇÒ ¸¸ÇÑ ±×¸®°í ½ÃÇèÇØ º¼¸¸ÇÑ ¹«¾ğ°¡¸¦ °¡Áú ÇÊ¿ä°¡ ÀÖ´Ù.

´ç½ÅÀÌ Ä¿¹Â´ÏÆ¼¸¦ Á¦ÀÛÇÏ±â ½ÃÀÛÇßÀ» ¶§, ´ç½ÅÀÌ Á¦½ÃÇÒ ¼ö ÀÖ´Â °ÍÀº 'Å¸´çÇÑ ¾à¼Ó'ÀÌ´Ù. ´ç½ÅÀÇ ÇÁ·Î±×·¥Àº Æ¯Á¤ÀûÀ¸·Î Àß µ¹¾Æ°¥ ÇÊ¿ä°¡ ¾ø´Ù. ´ëÃæ ¸¸µé°Å³ª, ¹ö±×°¡ ¸¹°Å³ª, ºÒ¿ÏÀüÇÏ°Å³ª, ±×¸®°í Àß ¹®¼­·Î ÀÛ¼ºµÇÁö ¾ÊÀ» ¼ö ÀÖ´Ù. ½ÇÆĞÇÏÁö ¸»¾Æ¾ßÇÏ´Â °ÍÀº (a) µ¹¾Æ°¡´Â °Í, ±×¸®°í (b) °¡´É¼ºÀÌ ÀÖ´Â °øµ¿ °³¹ßÀÚµé¿¡°Ô ÀÌ°ÍÀÌ ¿¹Ãø°¡´ÉÇÑ ¹Ì·¡¿¡ ºĞ¸íÈ÷ ´ÜÁ¤ÇÏ°Ô µÉ °ÍÀ¸·Î ¹ßÀü ÇÒ ¼ö ÀÖ´Ù°í ¼³µæÇÏ´Â °ÍÀÌ´Ù.

¸®´ª½º¿Í ÀÎÃâ ¸ŞÀÏ ¸ğµÎ °­ÇÏ°í ¸Å·ÂÀûÀÎ ±âº» ¼³°è·Î¼­ °ø°øÀÌ µÇ¾ú´Ù. ³»°¡ ¹ßÇ¥ÇÑ ¹Ù¿Í °°ÀÌ ½ÃÀå¸ğµ¨À» »ı°¢ÇÏ´Â ¸¹Àº »ç¶÷µéÀº ÀÌ°ÍÀÌ ÀÌ·¯ÇÑ À§ÇèÀ» Á¤È®È÷ °í·ÁÇÏ°í ÀÖ°í, ±×¸®°í ³ôÀº ¼öÁØÀÇ ¼³°è Á÷°ü°ú ÇÁ·ÎÁ§Æ® ¸®´õÀÇ ¸í¼®ÇÔÀº ´ëÃ¼ ºÒ°¡ÀûÀÌ¶ó´Â °á·Ğ¿¡ µµ´ŞÇÏ¿´´Ù.

±×·¯³ª ¸®´ª½º´Â À¯´Ğ½º·ÎºÎÅÍ ±×ÀÇ ¼³°è¸¦ °¡Á®¿Ô´Ù. ³ª´Â Ã³À½¿¡ Àü·¡ÀÇ popclient·Î ºÎÅÍ ³» °ÍÀ» °¡Á®¿Ô´Ù. (ºñ·Ï ÀÌ°ÍÀÌ ³ªÁß¿¡ ¸®´ª½º°¡ °ÅÃÄ¿Ô´ø °Íº¸´Ù ¸¹Àº ¾çÀÇ ÀÌ¾ß±â¸¦ ¹Ù²Ù¾ú´õ¶óµµ). µû¶ó¼­, ½ÃÀå ½ºÅ¸ÀÏ¿¡¼­ÀÇ ¸®´õ/ÁøÇàÀÚ´Â ¸Å¿ì ¶Ù¾î³­ ¼³°è ´É·ÂÀ» °¡Áö°í ÀÖ¾î¾ß ÇÏ´Â °ÍÀÎ°¡? ¶Ç´Â ±×°¡ ´Ù¸¥ »ç¶÷µéÀÇ ¼³°è Àç´ÉÀ» ¿ÜºÎ·ÎºÎÅÍ °¡Á®¿Ã ¼ö ÀÖ¾î¾ß ÇÏ´Â °ÍÀÎ°¡?
³ª´Â Á¶Á¤ÀÚ°¡ ¸Å¿ì ¶Ù¾î³­ ¼öÁØÀÇ ¼³°è¸¦ ¸¸µé¾î³¾ ¼ö ÀÖ´Â °ÍÀº Áß¿äÇÏÁö ¾Ê´Ù°í »ı°¢ÇÑ´Ù, ±×·¯³ª Á¶Á¤ÀÚ°¡ ´Ù¸¥ »ç¶÷À¸·ÎºÎÅÍ ÁÁÀº ¼³°è ¾ÆÀÌµğ¾î¸¦ ÀÎÁöÇÒ ÁÙ ¾Æ´Â ´É·ÂÀº ¸Å¿ì Áß¿äÇÏ´Ù°í »ı°¢ÇÑ´Ù.

¸®´ª½º¿Í ÀÎÃâ ¸ŞÀÏ ÇÁ·ÎÁ§Æ® ¸ğµÎ ÀÌ·¯ÇÑ Áõ°Å¸¦ º¸¿©ÁÖ°í ÀÖ´Ù. ¾Õ¼­ ¾ğ±ŞµÈ ¸Å¿ì ¾öÃ»³­ µ¶Ã¢ÀûÀÎ ¼³°èÀÚ°¡ ¾Æ´Ñ ¸®´ª½º´Â ÁÁÀº ¼³°è¸¦ ¾Ë¾Æº¸´Â °Í°ú ±×°ÍÀ» ¸®´ª½º Ä¿³Î¿¡ ÅëÇÕ½ÃÅ°´Â ¾öÃ»³­ ÀçÁÖ¸¦ º¸¿©ÁÖ¾ú´Ù. ±×¸®°í ³ª´Â ÀÎÃâ ¸ŞÀÏ¿¡¼­ÀÇ ÇÏ³ªÀÇ ¸Å¿ì °­·ÂÇÑ ¼³°è ¾ÆÀÌµğ¾î(SMTP Æ÷¿öµù)°¡ ´©±º°¡ ·ÎºÎÅÍ ¾î¶»°Ô ³ª¿À´ÂÁö ÀÌ¹Ì ¼³¸íÇØ¿Ô´Ù.

ÀÌ ³í¹®ÀÇ ÃÊ±â µ¶ÀÚµéÀº ³»°¡ ³ª ½º½º·Î ¸¹Àº °ÍÀ» °¡Áö°í ÀÖ±â ¶§¹®¿¡, ½ÃÀå ÇÁ·ÎÁ§Æ®¿¡¼­ ¼³°è ¿øÁ¶¼ºÀ» °ú¼ÒÆò°¡ÇÏ±â ½¬¿ì¸ç, µû¶ó¼­ ±× Á¡À» ¿°µÎÇØ¾ß ÇÑ´Ù°í Á¦¾È ÇÔÀ¸·Î¼­ ³ª¸¦ °İ·ÁÇß´Ù. ÀÌ´Â Áø½ÇÀÏ ¼öµµ ÀÖ´Ù; (ÄÚµù ¶Ç´Â µğ¹ö±ë°ú´Â ¹İ´ë·Î) ¼³°è´Â ³ªÀÇ °¡Àå °­·ÂÇÑ ±â¼úÀÌ´Ù.

±×·¯³ª ¼ÒÇÁÆ®¿ş¾î µğÀÚÀÎ¿¡ ¸í¼®ÇÏ°í µ¶Ã¢ÀûÀÌ°Ô µÇ´Â ¹®Á¦´Â ½À°üÀÌ µÈ´Ù´Â °ÍÀÌ´Ù-´ç½ÅÀÌ ±×°ÍµéÀ» Æ°Æ°ÇÏ°í °£´ÜÇÏ°Ô À¯ÁöÇØ¾ßÇÒ ¶§ ´ç½ÅÀº ¹İ»çÀûÀ¸·Î ±×°ÍµéÀ» ¾öÃ»³ª°í º¹ÀâÇÏ°Ô ¸¸µé¾î ¹ö¸®±â ½ÃÀÛÇÑ´Ù. ³ª´Â ³»°¡ ÀÌ·¯ÇÑ ½Ç¼ö¸¦ Çß±â ¶§¹®¿¡ ÇÁ·ÎÁ§Æ®µéÀÌ ½ÇÆĞÇÏ¿´¾úÁö¸¸, ÀÎÃâ ¸ŞÀÏ¿¡ ÀÖ¾î¼­´Â ÀÌÁ¡À» ÇÇÇÏ±â·Î ´ÙÁüÇß´Ù.

µû¶ó¼­ ³ª´Â ³»°¡ ¸í¼®ÇØ Áö·Á´Â ³ªÀÇ °æÇâÀ» ¾ïÁ¦Çß±â ¶§¹®¿¡, ÀÎÃâ ¸ŞÀÏÀÌ ºÎºĞÀûÀ¸·Î ¼º°øÇß´Ù°í ¹Ï´Â´Ù; ÀÌ°ÍÀº (Àû¾îµµ) ¼º°øÀûÀÎ ½ÃÀå ÇÁ·ÎÁ§Æ®µéÀ» À§ÇØ¼­ ¼³°è µ¶Ã¢¼º¿¡ ¹İÇÑ´Ù°í ÁÖÀåÇÑ´Ù. ±×¸®°í ¸®´ª½ºÀÇ °æ¿ì¸¦ »ı°¢ÇØº¸¶ó. ¸®´ª½º Åä¹ßÁî°¡ °³¹ß Áß¿¡ ¿î¿µÃ¼Á¦ ¼³°è¿¡ ÀÖ¾î¼­ ±Ùº»ÀûÀÎ Çõ½ÅµéÀ» Çõ½Å ½ÃÄÑ¿Ô´Ù°í »ı°¢ÇØº¸ÀÚ; ¿ì¸®°¡ ÀÌ¹Ì °¡Áö°íÀÖ´Â °ÍÃ³·³ Ä¿³ÎÀÌ ¾ÈÀüÇÏ°í ¼º°øÀûÀÌ°Ô µÇ¾úÀ»±î?

¼³°è¿Í ÄÚµù¿¡ ´ëÇÑ ÀÏÁ¤ ±âº»ÀûÀÎ ¼öÁØÀº ¹°·Ğ ÇÊ¿äÇÏ´Ù, ±×·¯³ª ³ª´Â ½ÃÀå ³ë·ÂÀ» ±â¿ïÀÌ´Â °ÍÀ» ½É°¢ÇÏ°Ô »ı°¢ÇÏ´Â °ÅÀÇ ´ëºÎºĞÀÇ »ç¶÷µéÀÌ ±× ´É·ÂÀÇ ÃÖ¼ÒÄ¡¸¦ ÀÌ¹Ì ´É°¡ÇÑ´Ù°í »ı°¢ÇÑ´Ù. ¸í¼º ÀÖ´Â ¿ÀÇÂ¼Ò½º Ä¿¹Â´ÏÆ¼ÀÇ ³»ºÎ ½ÃÀåÀº °úÁ¤À» µû¶ó°¡Áö ¸øÇÒ Á¤µµ·Î À¯´ÉÇÏÁö ¾Ê´Â »ç¶÷µéÀÌ °³¹ß ³ë·ÂÀ» ±â¿ïÀÌÁö ¾Êµµ·Ï »ç¶÷µé¿¡°Ô ¹Ì¹¦ÇÑ ¾Ğ·ÂÀ» Çà»çÇÑ´Ù. Áö±İ±îÁö ÀÌ°ÍÀÌ ²Ï Àß Àû¿ëµÇ°í ÀÖ´Â °ÍÃ³·³ º¸ÀÎ´Ù.

³»°¡ ½ÃÀå ÇÁ·ÎÁ§Æ®¿¡ ÀÖ¾î¼­ ¶Ù¾î³­ ¼³°è ´É·Â¸¸Å­ÀÌ³ª Áß¿äÇÑ-±×¸®°í ´õ¿í Áß¿äÇÒÁöµµ ¸ğ¸£´Â, ¼ÒÇÁÆ®¿ş¾î °³¹ß°ú ¿¬°üµÇÁö ¾ÊÀº ¶Ç´Ù¸¥ Á¾·ùÀÇ ±â¼úÀÌ ÀÖ´Ù. ½ÃÀå ÇÁ·ÎÁ§Æ®ÀÇ Á¶Á¤ÀÚ³ª ¸®´õ´Â ¹İµå½Ã ÁÁÀº »ç¶÷µé°ú ÀÇ»ç¼ÒÅë ´É·ÂÀ» °¡Áö°í ÀÖ¾î¾ß ÇÑ´Ù.

ÀÌ´Â ¸Å¿ì ¸íÈ®ÇØ¾ß ÇÑ´Ù. °³¹ß Ä¿¹Â´ÏÆ¼¸¦ ¸¸µé±â À§ÇØ¼­´Â, ´ç½ÅÀº »ç¶÷µéÀ» ²ø¾îµéÀÌ°í, ´ç½ÅÀÌ Áö±İ ÇÏ´Â ÀÏ¿¡ Èï¹Ì¸¦ ºÙÀÌ°Ô ¸¸µé°í, ±×¸®°í ±×µéÀÌ ÇÏ´Â ÀÏÀÇ ¾ç¿¡ ´ëÇØ ±×µéÀÌ Çàº¹À» ´À³¢°Ô²û ÇØ¾ß ÇÑ´Ù. ±â¼úÀûÀ¸·Î ÁÁÀº ¼ºÀûÀ» ¿Ã¸®´Â °ÍÀº ÀÌÁ¡À» ¼ºÃëÇÏ´Â ¹æÇâÀ¸·Î ³ª¾Æ°¥ °ÍÀÌÁö¸¸, ÀüÃ¼ ½ºÅä¸®¿Í´Â °Å¸®°¡ ¸Ö´Ù. ´ç½ÅÀÌ ³ªÅ¸³»´Â ¼º°İ ¶ÇÇÑ ¹®Á¦°¡ µÈ´Ù.

¸®´ª½º°¡ »ç¶÷µéÀÌ ±×¸¦ ÁÁ¾ÆÇÏ°í ±×¸¦ µµ¿ÍÁÖ´Â °ÍÀ» ÁÁ¾ÆÇÏ´Â °ÍÀº ¿ì¿¬ÀÇ ÀÏÄ¡°¡ ¾Æ´Ï´Ù. ³»°¡ ´ëÁß°ú ÀÏÇÏ°í ¿ø¸Ç¼î ÄÚ¹ÌµğÀÇ ±³ÈÆ°ú ÀÚ±ØÀ» ¹Ş°í ÀÖ´Â È°±â ³ÑÄ¡´Â ¿ÜÇâÀûÀÎ »ç¶÷ÀÎ °ÍÀº ¿ì¿¬ÀÇ ÀÏÄ¡°¡ ¾Æ´Ï´Ù. ½ÃÀå ¸ğµ¨ÀÌ Àß µ¹¾Æ°¡°Ô ÇÏ±â À§ÇØ¼­´Â, ´ç½ÅÀÌ ÃÖ¼ÒÇÑ »ç¶÷µéÀ» ±â»Ú°Ô ÇÏ´Â °Í¿¡ ¾ÆÁÖ ÀÛÀº ´É·ÂÀÌ¶óµµ ÀÖ´ÂÁö´Â ¾öÃ»³­ µµ¿òÀÌ µÈ´Ù.


## ¿ÀÇÂ¼Ò½º ¼ÒÇÁÆ®¿ş¾î¿¡ ´ëÇÑ »çÈ¸Àû ¹®¸Æ

ÀÌ´Â ºĞ¸íÈ÷ ±â·ÏµÈ´Ù: ÃÖ°íÀÇ hack´Â ±Û¾´ÀÌÀÇ ÀÏ»ó»ıÈ° ¹®Á¦µé¿¡ ´ëÇÑ °³ÀÎÀûÀÎ ÇØ°áÃ¥À¸·Î ½ÃÀÛÇØ¼­ ÆÛÁø´Ù, ¿Ö³ÄÇÏ¸é ±× ¹®Á¦´Â »ç¿ëÀÚµéÀÇ ´ë±Ô¸ğ Áı´Ü¿¡ ´ëÇÑ ÀüÇüÀûÀÎ ¹®Á¦·Î º¯¸ğÇÏ±â ¶§¹®ÀÌ´Ù.

18. Èï¹Ì·Î¿î ¹®Á¦¸¦ ÇØ°áÇÏ±â À§ÇØ¼­´Â, ´ç½ÅÀÌ Èï¹Ì¸¦ ´À³¢´Â ¹®Á¦ºÎÅÍ Ã£¾Æº¸±â ½ÃÀÛÇÏ¶ó

Ä® ÇØ¸®½º¿Í ¿¾³¯ÀÇ ÆËÅ¬¶óÀÌ¾ğÆ®(popclient), ±×¸®°í ³ª¿Í ÀÎÃâ ¸ŞÀÏÀÌ ±×·¯Çß´Ù. ±×·¯³ª ÀÌ´Â ¿À·£ ±â°£µ¿¾È ÀÌÇØµÇ¾î ¿Ô´Ù. Èï¹Ì·Î¿î Á¡, ¸®´ª½º¿Í ÀÎÃâ ¸ŞÀÏÀÇ ¿ª»ç°¡ ¿ì¸®·Î ÇÏ¿©±İ ÁıÁßÇÏ°Ô ²û ÇÏ´Â Á¡Àº ¹Ù·Î ´ÙÀ½ ´Ü°èÀÌ´Ù-»ç¿ëÀÚ¿Í °øµ¿ °³¹ßÀÚÀÇ Å©°í È°¹ßÇÑ Ä¿¹Â´ÏÆ¼ÀÇ Á¸Àç ¾È¿¡¼­ÀÇ ¼ÒÇÁÆ®¿ş¾îÀÇ ¹ßÀüÀÌ´Ù.

The Mythical Man-Month¿¡¼­´Â, ÇÁ·¹µå ºê·è½º´Â ÇÁ·Î±×·¡¸Ó ½Ã°£ÀÌ ´ëÃ¼ °¡´ÉÇÏÁö ¾Ê´Ù°í ÁÖÀåÇÑ´Ù; ³ªÁß¿¡ ¼ÒÇÁÆ®¿ş¾î ÇÁ·ÎÁ§Æ®¿¡ °³¹ßÀÚ¸¦ ÅõÀÔÇÏ´Â °ÍÀº ÀÌ¸¦ ´Ê°Ô ¸¸µç´Ù. ¿ì¸®°¡ ¾Õ¼­ ºÁ¿Â ¹Ù¿Í °°ÀÌ, ±×´Â ÀÛ¾÷·®Àº ¿ÀÁ÷ ÀÏÁ÷¼±»óÀ¸·Î Áõ°¡ÇÔ°ú ´Ş¸®, ÇÁ·ÎÁ§Æ®ÀÇ º¹Àâµµ¿Í ÀÇ»ç¼ÒÅë ºñ¿ëÀº °³¹ßÀÚ ¼öÀÇ Á¦°öÀ¸·Î Áõ°¡ÇÑ´Ù°í ÁÖÀåÇÑ´Ù. ºê·è½ºÀÇ ¹ıÄ¢Àº Áø¸®·Î¼­ ³Î¸® ¹Ş¾Æµé¿©Áø´Ù. ±×·¯³ª ¿ì¸®´Â ÀÌ ³í¹®¿¡¼­ ¿ÀÇÂ¼Ò½º °³¹ßÀÇ °úÁ¤¿¡ ÀÖ¾î¼­ ¼ö¸¹Àº ¹æ¹ıµéÀÌ ÀÌ°Í¿¡ ¼û°ÜÁø °¡Á¤¿¡ ¾î±ß³²À» º¸¾Ò´Ù-±×¸®°í, °æÇè¿¡ ±âÀÎÇÏ¿©, ¸¸¾à¿¡ ºê·è½ºÀÇ ¹ıÄ¢ÀÌ ÀüÃ¼ Æ²ÀÌ¾ú´Ù¸é ¸®´ª½º´Â ºÒ°¡´ÉÇßÀ» °ÍÀÓÀ» ¾Ë¾Ò´Ù.

Á¦·Ñµå ¿şÀÎ¹ö±×ÀÇ ÀüÇüÀûÀÎ 'ÄÄÇ»ÅÍ ÇÁ·Î±×·¡¹ÖÀÇ ½É¸®ÇĞ(The Psychology of Computer Programming)'Àº, Áö³ª°í ³ª¼­ º¸´Ï, ºê·è½º¿¡ ´ëÇÑ ÇÊ¿äÇÑ Á¤Á¤À» ¹«¾ùÀ¸·Î º¼ ¼ö ÀÖ´ÂÁö¸¦ Á¦¾ÈÇÑ´Ù. "°´°üÈ­ ÇÁ·Î±×·¡¹Ö(egoless programming)"¿¡ ´ëÇÑ ±×ÀÇ ÁÖÀå¿¡ µû¸£¸é, ¿şÀÎ¹ö±×´Â, °³¹ßÀÚµéÀÌ ±×µéÀÇ ÄÚµå¿¡ ´ëÇØ ÅÔ¼¼¸¦ ºÎ¸®Áö ¾Ê°í, ´Ù¸¥ »ç¶÷µé·Î ÇÏ¿©±İ ±× ¾È¿¡ ÀÖ´Â ¹ö±×¿Í °¡´É¼º ÀÖ´Â Çâ»óÀ» º¸µµ·Ï °İ·ÁÇÏ´Â Á÷Àå¿¡¼­´Â, Çâ»óÀÌ ´Ù¸¥ ¾î¶² °÷¿¡¼­ º¸´Ù ¸Å¿ì ºü¸£°Ô ³ªÅ¸³­´Ù°í ÁÖÀåÇÑ´Ù. (ÃÖ±Ù¿¡, ÄÚµå Á¦ÀÛÀÚµéÀÌ Â¦À» ÀÌ·ç¾î ´Ù¸¥ »ç¶÷ °ÍµéÀ» º¸µµ·Ï °í¿ëÇÏ´Â, ÄËÆ® º¤ÀÇ '±ØµµÀÇ ÇÁ·Î±×·¡¹Ö (extreme programming)'Àº ÀÌ·¯ÇÑ È¿°ú¸¦ °­È­ÇÏ±â À§ÇÑ ½Ãµµ·Î º¸¿©Áø´Ù.)

Àü¹® ¿ë¾îÀÇ ¿şÀÎ¹ö±×ÀÇ ¼±ÅÃÀº ¾Æ¸¶ ±×ÀÇ ºĞ¼®À¸·Î ÇÏ¿©±İ ÀÌ°ÍÀÌ ¹ŞÀ» ¸¸ÇÑ Çã°¡¸¦ ¾ò´Â °ÍÀ» ¸·°íÀÖ´Ù-ÇÑ »ç¶÷Àº ¹İµå½Ã ÀÎÅÍ³İ ÇØÄ¿µéÀ» "°´°üÀû" ÀÌ¶ó°í ¹¦»çÇÏ´Â »ı°¢¿¡ µ¿ÀÇ¸¦ ÇØ¾ß ÇÑ´Ù. ±×·¯³ª ³ª´Â ±×ÀÇ ÁÖÀåÀÌ ¾î¶² °Íº¸´Ù ´õ¿í ´õ °­·ÂÇÏ´Ù°í ¹Ï´Â´Ù.

"°´°üÈ­ ÇÁ·Î±×·¡¹Ö" È¿°úÀÇ ¿ÏÀüÇÑ ÈûÀ» ÀÌ¿ëÇÏ´Â ½ÃÀå ¹æ¹ıÀº ºê·è½ºÀÇ ¹ıÄ¢À» ¿Ïº®È÷ ¿ÏÈ­ÇÑ´Ù. ºê·è½ºÀÇ ¹ıÄ¢¿¡ ¼û°ÜÁø ¿ø¸®µéÀº »ç¶óÁöÁö´Â ¾ÊÁö¸¸, ´ë±Ô¸ğÀÇ °³¹ßÀÚ ÀÎ±¸¿Í ºÎ½ÇÇÑ ÀÇ»ç¼ÒÅëÀÌ ÁÖ¾îÁ³À» ¶§´Â ÀÌ°ÍÀÇ È¿°ú´Â ´Ù¸¥ ¹æ¹ıµé·Î´Â º¼ ¼ö ¾ø´Â °æÀïÀûÀÎ ºñ¼±Çü¼º¿¡ ÀÇÇØ ¾ĞµµµÉ ¼ö ÀÖ´Ù. ÀÌ Á¡Àº ´ºÅÏ°ú ¾ÆÀÌ½´Å¸ÀÎÀÇ ¹°¸®ÇĞ »çÀÌÀÇ °ü°è¿Í ºñ½ÁÇÏ´Ù-¿¹ÀüÀÇ ½Ã½ºÅÛÀº ÀÛÀº ¿¡³ÊÁö·Î ¿©ÀüÈ÷ À¯È¿ÇÏÁö¸¸, ¸¸¾à ´ç½ÅÀÌ ¾öÃ»³ª°Ô Å« ¾ç°ú ÃæºĞÈ÷ ºü¸¥ ¼Óµµ¸¦ ÅõÀÔÇÑ´Ù¸é ´ç½ÅÀº ÇÙ Æø¹ßÀÌ³ª ¸®´ª½º°¡ »ı±ä °ÍÃ³·³ ³î¶ó°Ô µÉ °ÍÀÌ´Ù.

À¯´Ğ½ºÀÇ ¿ª»ç´Â ¿ì¸®°¡ ¸®´ª½º ·ÎºÎÅÍ ¹è¿î °Í(±×¸®°í ¸®´ª½ºÀÇ ¹æ¹ıÀ» ±×´ë·Î µû¶ó ÇÔÀ¸·Î¼­ ¼Ò±Ô¸ğÀÇ ½ÇÇè¿¡¼­ Áõ¸íÇÑ °Í)À» ÁØºñ½ÃÄ×¾î¾ß Çß´Ù.
 That is, while coding
remains an essentially solitary activity, the really great hacks come from harnessing the attention and
brainpower of entire communities. The developer who uses only his or her own brain in a closed project is
going to fall behind the developer who knows how to create an open, evolutionary context in which
feedback exploring the design space, code contributions, bug-spotting, and other improvements come from
from hundreds (perhaps thousands) of people.

But the traditional Unix world was prevented from pushing this approach to the ultimate by several factors.
One was the legal contraints of various licenses, trade secrets, and commercial interests. Another (in
hindsight) was that the Internet wasn't yet good enough.

Before cheap Internet, there were some geographically compact communities where the culture encouraged
Weinberg's ''egoless'' programming, and a developer could easily attract a lot of skilled kibitzers and co-
developers. Bell Labs, the MIT AI and LCS labs, UC Berkeleyâ€”these became the home of innovations that
are legendary and still potent.

Linux was the first project for which a conscious and successful effort to use the entire world as its talent
pool was made. I don't think it's a coincidence that the gestation period of Linux coincided with the birth
of the World Wide Web, and that Linux left its infancy during the same period in 1993â€“1994 that saw the
takeoff of the ISP industry and the explosion of mainstream interest in the Internet. Linus was the first
person who learned how to play by the new rules that pervasive Internet access made possible.

While cheap Internet was a necessary condition for the Linux model to evolve, I think it was not by itself a
sufficient condition. Another vital factor was the development of a leadership style and set of cooperative
customs that could allow developers to attract co-developers and get maximum leverage out of the medium.

But what is this leadership style and what are these customs? They cannot be based on power
relationshipsâ€”and even if they could be, leadership by coercion would not produce the results we see.
Weinberg quotes the autobiography of the 19th-century Russian anarchist Pyotr Alexeyvich Kropotkin's
Memoirs of a Revolutionist to good effect on this subject:

Having been brought up in a serf -owner's family, I entered active life, like all young men of my time, with
a great deal of confidence in the necessity of commanding, ordering, scolding, punishing and the like. But
when, at an early stage, I had to manage serious enterprises and to deal with [free] men, and when each
mistake would lead at once to heavy consequences, I began to appreciate the difference between acting
on the principle of command and discipline and acting on the principle of common understanding. The
former works admirably in a military parade, but it is worth nothing where real life is concerned, and the
aim can be achieved only through the severe effort of many converging wills.

The ''severe effort of many converging wills'' is precisely what a project like Linux requiresâ€”and the
''principle of command'' is effectively impossible to apply among volunteers in the anarchist's paradise we


call the Internet. To operate and compete effectively, hackers who want to lead collaborative projects have
to learn how to recruit and energize effective communities of interest in the mode vaguely suggested by
Kropotkin's ''principle of understanding''. They must learn to use Linus's Law.[SP]

Earlier I referred to the ''Delphi effect'' as a possible explanation for Linus's Law. But more powerful analogies
to adaptive systems in biology and economics also irresistably suggest themselves. The Linux world behaves
in many respects like a free market or an ecology, a collection of selfish agents attempting to maximize
utility which in the process produces a self-correcting spontaneous order more elaborate and efficient than
any amount of central planning could have achieved. Here, then, is the place to seek the ''principle of
understanding''.

The ''utility function'' Linux hackers are maximizing is not classically economic, but is the intangible of their
own ego satisfaction and reputation among other hackers. (One may call their motivation ''altruistic'', but
this ignores the fact that altruism is itself a form of ego satisfaction for the altruist). Voluntary cultures that
work this way are not actually uncommon; one other in which I have long participated is science fiction
fandom, which unlike hackerdom has long explicitly recognized ''egoboo'' (ego-boosting, or the
enhancement of one's reputation among other fans) as the basic drive behind volunteer activity.

Linus, by successfully positioning himself as the gatekeeper of a project in which the development is mostly
done by others, and nurturing interest in the project until it became self-sustaining, has shown an acute
grasp of Kropotkin's ''principle of shared understanding''. This quasi-economic view of the Linux world
enables us to see how that understanding is applied.

We may view Linus's method as a way to create an efficient market in ''egoboo''â€”to connect the selfishness
of individual hackers as firmly as possible to difficult ends that can only be achieved by sustained
cooperation. With the fetchmail project I have shown (albeit on a smaller scale) that his methods can be
duplicated with good results. Perhaps I have even done it a bit more consciously and systematically than
he.

Many people (especially those who politically distrust free markets) would expect a culture of self-directed
egoists to be fragmented, territorial, wasteful, secretive, and hostile. But this expectation is clearly falsified
by (to give just one example) the stunning variety, quality, and depth of Linux documentation. It is a hallowed
given that programmers hate documenting; how is it, then, that Linux hackers generate so much
documentation? Evidently Linux's free market in egoboo works better to produce virtuous, other-directed
behavior than the massively-funded documentation shops of commercial software producers.

Both the fetchmail and Linux kernel projects show that by properly rewarding the egos of many other
hackers, a strong developer/coordinator can use the Internet to capture the benefits of having lots of co-
developers without having a project collapse into a chaotic mess. So to Brooks's Law I counter-propose the
following:


19: Provided the development coordinator has a communications medium at least as good as the Internet,
and knows how to lead without coercion, many heads are inevitably better than one.

I think the future of open-source software will increasingly belong to people who know how to play Linus's
game, people who leave behind the cathedral and embrace the bazaar. This is not to say that individual
vision and brilliance will no longer matter; rather, I think that the cutting edge of open-source software will
belong to people who start from individual vision and brilliance, then amplify it through the effective
construction of voluntary communities of interest.

Perhaps this is not only the future of open-source software. No closed-source developer can match the
pool of talent the Linux community can bring to bear on a problem. Very few could afford even to hire the
more than 200 (1999: 600, 2000: 800) people who have contributed to fetchmail!

Perhaps in the end the open-source culture will triumph not because cooperation is morally right or software
''hoarding'' is morally wrong (assuming you believe the latter, which neither Linus nor I do), but simply
because the closed-source world cannot win an evolutionary arms race with open-source communities that
can put orders of magnitude more skilled time into a problem.


