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

Linus Torvalds's style of development—release early and often, delegate everything you can, be open to
the point of promiscuity—came as a surprise. No quiet, reverent cathedral-building here—rather, the Linux
community seemed to resemble a great babbling bazaar of differing agendas and approaches (aptly
symbolized by the Linux archive sites, who'd take submissions from anyone) out of which a coherent and
stable system could seemingly emerge only by a succession of miracles.

The fact that this bazaar style seemed to work, and work well, came as a distinct shock. As I learned my
way around, I worked hard not just at individual projects, but also at trying to understand why the Linux
world not only didn't fly apart in confusion but seemed to go from strength to strength at a speed barely
imaginable to cathedral-builders.

By mid-1996 I thought I was beginning to understand. Chance handed me a perfect way to test my theory,
in the form of an open-source project that I could consciously try to run in the bazaar style. So I did—and
it was a significant success.

This is the story of that project. I'll use it to propose some aphorisms about effective open-source
development. Not all of these are things I first learned in the Linux world, but we'll see how the Linux world
gives them particular point. If I'm correct, they'll help you understand exactly what it is that makes the Linux
community such a fountain of good software—and, perhaps, they will help you become more productive
yourself.


## The Mail Must Get Through

Since 1993 I'd been running the technical side of a small free-access Internet service provider called Chester
County InterLink (CCIL) in West Chester, Pennsylvania. I co-founded CCIL and wrote our unique multiuser
bulletin-board software—you can check it out by telnetting to locke.ccil.org. Today it supports almost three
thousand users on thirty lines. The job allowed me 24-hour-a-day access to the net through CCIL's 56K
line—in fact, the job practically demanded it!

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
nor love. But not in the Linux world—which may explain why the average quality of software originated in
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
went away or was completely rewritten—but while it was there, it provided scaffolding for the infant that
would eventually become Linux.

In the same spirit, I went looking for an existing POP utility that was reasonably well coded, to use as a
development base.

The source-sharing tradition of the Unix world has always been friendly to code reuse (this is why the GNU
project chose Unix as a base OS, in spite of serious reservations about the OS itself). The Linux world has
taken this tradition nearly to its technological limit; it has terabytes of open sources generally available. So
spending time looking for some else's almost-good-enough is more likely to give you good results in the
Linux world than anywhere else.

And it did for me. With those I'd found earlier, my second search made up a total of nine candidates—
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
developed this way. The Lisp library, in effect, was not—because there were active Lisp archives outside the
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
rewarded—stimulated by the prospect of having an ego-satisfying piece of the action, rewarded by the
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

In the bazaar view, on the other hand, you assume that bugs are generally shallow phenomena—or, at least,
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
appears that what Linus has shown is that this applies even to debugging an operating system—that the
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

One key to understanding is to realize exactly why it is that the kind of bug report non–source-aware users
normally turn in tends not to be very useful. Non–source-aware users tend to report only surface symptoms;


they take their environment for granted, so they (a) omit critical background data, and (b) seldom include
a reliable recipe for reproducing the bug.

The underlying problem here is a mismatch between the tester's and the developer's mental models of the
program; the tester, on the outside looking in, and the developer on the inside looking out. In closed-
source development they're both stuck in these roles, and tend to talk past each other and find each other
deeply frustrating.

Open-source development breaks this bind, making it far easier for tester and developer to develop a
shared representation grounded in the actual source code and to communicate effectively about it.
Practically, there is a huge difference in leverage for the developer between the kind of bug report that just
reports externally-visible symptoms and the kind that hooks directly to the developer's source-code–based
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

There are are still more reasons that source-code–level bug reporting tends to be very efficient. They center
around the fact that a single error can often have multiple possible symptoms, manifesting differently
depending on details of the user's usage pattern and environment. Such errors tend to be exactly the sort
of complex and subtle bugs (such as dynamic-memory-management errors or nondeterministic interrupt-
window artifacts) that are hardest to reproduce at will or to pin down by static analysis, and which do the
most to create long-term problems in software.

A tester who sends in a tentative source-code–level characterization of such a multi-symptom bug (e.g. "It
looks to me like there's a window in the signal handling near line 1250" or "Where are you zeroing that
bu ffer?") may give a developer, otherwise too close to the code to see it, the critical clue to a half-dozen
disparate symptoms. In cases like this, it may be hard or even impossible to know which externally-visible
misbehaviour was caused by precisely which bug—but with frequent releases, it's unnecessary to know.
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
effective than a few people running traces sequentially—even if the few have a much higher average skill
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
might be in order—after all, it wasn't just a POP client any more. But I hesitated, because there was as yet
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
was serviceable but grubby—inelegant and with too many exiguous options hanging out all over. The
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
methods. A user gave me this terrific idea—all I had to do was understand the implications.

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

When you hit a wall in development—when you find yourself hard put to think past the next patch—it's
often time to ask not whether you've got the right answer, but whether you're asking the right question.
Perhaps the problem needs to be reframed.

Well, I had reframed my problem. Clearly, the right thing to do was (1) hack SMTP forwarding support into
the generic driver, (2) make it the default mode, and (3) eventually throw out all the other delivery modes,
especially the deliver-to -file and deliver-to -standard-output options.

I hesitated over step 3 for some time, fearing to upset long-time popclient users dependent on the alternate
delivery mechanisms. In theory, they could immediately switch to .forward files or their non-sendmail
equivalents to get the same effects. In practice the transition might have been messy.

But when I did it, the benefits proved huge. The cruftiest parts of the driver code vanished. Configuration
got radically simpler—no more grovelling around for the system MDA and user's mailbox, no more worries
about whether the underlying OS supports file locking.


Also, the only way to lose mail vanished. If you specified delivery to a file and the disk got full, your mail
got lost. This can't happen with SMTP forwarding because your SMTP listener won't return OK unless the
message can be delivered or at least spooled for later delivery.

Also, performance improved (though not so you'd notice it in a single run). Another not insignificant benefit
of this change was that the manual page got a lot simpler.

Later, I had to bring delivery via a user-specified local MDA back in order to allow handling of some obscure
situations involving dynamic SLIP. But I found a much simpler way to do it.

The moral? Don't hesitate to throw away superannuated features when you can do it without loss of
effectiveness. Antoine de Saint-Exup�ry (who was an aviator and aircraft designer when he wasn't authoring
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
become special cases of debugging—fixing 'bugs of omission' in the original capabilities or concept of the
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


With the SMTP forwarding feature, it pulled far enough in front of the competition to potentially become
a ''category killer'', one of those classic programs that fills its niche so competently that the alternatives are
not just discarded but almost forgotten.

I think you can't really aim or plan for a result like this. You have to get pulled into it by design ideas so
powerful that afterward the results just seem inevitable, natural, even foreordained. The only way to try for
ideas like that is by having lots of ideas—or by having the engineering judgment to take other peoples'
good ideas beyond where the originators thought they could go.

Andy Tanenbaum had the original idea to build a simple native Unix for IBM PCs, for use as a teaching tool
(he called it Minix). Linus Torvalds pushed the Minix concept further than Andrew probably thought it could
go—and it grew into something wonderful. In the same way (though on a smaller scale), I took some ideas
by Carl Harris and Harry Hochheiser and pushed them hard. Neither of us was 'original' in the romantic
way people think is genius. But then, most science and engineering and software development isn't done
by original genius, hacker mythology to the contrary.

The results were pretty heady stuff all the same—in fact, just the kind of success every hacker lives for! And
they meant I would have to set my standards even higher. To make fetchmail as good as I now saw it could
be, I'd have to write not just for my own needs, but also include and support features necessary to others
but outside my orbit. And do that while keeping the program simple and robust.

The first and overwhelmingly most important feature I wrote after realizing this was multidrop support—
the ability to fetch mail from mailboxes that had accumulated all mail for a group of users, and then route
each piece of mail to its individual recipients.

I decided to add the multidrop support partly because some users were clamoring for it, but mostly because
I thought it would shake bugs out of the single-drop code by forcing me to deal with addressing in full
generality. And so it proved. Getting RFC 822 address parsing right took me a remarkably long time, not
because any individual piece of it is hard but because it involved a pile of interdependent and fussy details.

But multidrop addressing turned out to be an excellent design decision as well. Here's how I knew:

14. Any tool should be useful in the expected way, but a truly great tool lends itself to uses you never
expected.

The unexpected use for multidrop fetchmail is to run mailing lists with the list kept, and alias expansion
done, on the client side of the Internet connection. This means someone running a personal machine
through an ISP account can manage a mailing list without continuing access to the ISP's alias files.

Another important change demanded by my beta-testers was support for 8-bit MIME (Multipurpose Internet
Mail Extensions) operation. This was pretty easy to do, because I had been careful to keep the code 8-bit
clean (that is, to not press the 8th bit, unused in the ASCII character set, into service to carry information


within the program). Not because I anticipated the demand for this feature, but rather in obedience to
another rule:

15. When writing gateway software of any kind, take pains to disturb the data stream as little as possible—
and never throw away information unless the recipient forces you to!

Had I not obeyed this rule, 8-bit MIME support would have been difficult and buggy. As it was, all I had to
do is read the MIME standard (RFC 1652) and add a trivial bit of header-generation logic.

Some European users bugged me into adding an option to limit the number of messages retrieved per
session (so they can control costs from their expensive phone networks). I resisted this for a long time, and
I'm still not entirely happy about it. But if you're writing for the world, you have to listen to your customers—
this doesn't change just because they're not paying you in money.

## A Few More Lessons from Fetchmail

Before we go back to general software-engineering issues, there are a couple more specific lessons from
the fetchmail experience to ponder. Nontechnical readers can safely skip this section.

The rc (control) file syntax includes optional 'noise' keywords that are entirely ignored by the parser. The
English-like syntax they allow is considerably more readable than the traditional terse keyword-value pairs
you get when you strip them all out.

These started out as a late-night experiment when I noticed how much the rc file declarations were
beginning to resemble an imperative minilanguage. (This is also why I changed the original popclient
''server'' keyword to ''poll'').

It seemed to me that trying to make that imperative minilanguage more like English might make it easier
to use. Now, although I'm a convinced partisan of the ''make it a language'' school of design as exemplified
by Emacs and HTML and many database engines, I am not normally a big fan of ''English-like'' syntaxes.

Traditionally programmers have tended to favor control syntaxes that are very precise and compact and
have no redundancy at all. This is a cultural legacy from when computing resources were expensive, so
parsing stages had to be as cheap and simple as possible. English, with about 50% redundancy, looked like
a very inappropriate model then.

This is not my reason for normally avoiding English-like syntaxes; I mention it here only to demolish it. With
cheap cycles and core, terseness should not be an end in itself. Nowadays it's more important for a language
to be convenient for humans than to be cheap for the computer.

There remain, however, good reasons to be wary. One is the complexity cost of the parsing stage—you
don't want to raise that to the point where it's a significant source of bugs and user confusion in itself.
Another is that trying to make a language syntax English-like often demands that the ''English'' it speaks


be bent seriously out of shape, so much so that the superficial resemblance to natural language is as
confusing as a traditional syntax would have been. (You see this bad effect in a lot of so-called ''fourth
generation'' and commercial database-query languages.)

The fetchmail control syntax seems to avoid these problems because the language domain is extremely
restricted. It's nowhere near a general-purpose language; the things it says simply are not very complicated,
so there's little potential for confusion in moving mentally between a tiny subset of English and the actual
control language. I think there may be a broader lesson here:

16. When your language is nowhere near Turing-complete, syntactic sugar can be your friend.

Another lesson is about security by obscurity. Some fetchmail users asked me to change the software to
store passwords encrypted in the rc file, so snoopers wouldn't be able to casually see them.

I didn't do it, because this doesn't actually add protection. Anyone who's acquired permissions to read your
rc file will be able to run fetchmail as you anyway—and if it's your password they're after, they'd be able to
rip the necessary decoder out of the fetchmail code itself to get it.

All .fetchmailrc password encryption would have done is give a false sense of security to people who don't
think very hard. The general rule here is:

17. A security system is only as secure as its secret. Beware of pseudo-secrets.

## Necessary Preconditions for the Bazaar Style

Early reviewers and test audiences for this essay consistently raised questions about the preconditions for
successful bazaar-style development, including both the qualifications of the project leader and the state of
code at the time one goes public and starts to try to build a co-developer community.

It's fairly clear that one cannot code from the ground up in bazaar style [IN]. One can test, debug and
improve in bazaar style, but it would be very hard to originate a project in bazaar mode. Linus didn't try it.
I didn't either. Your nascent developer community needs to have something runnable and testable to play
with.

When you start community-building, what you need to be able to present is a plausible promise. Yo ur
program doesn't have to work particularly well. It can be crude, buggy, incomplete, and poorly documented.
What it must not fail to do is (a) run, and (b) convince potential co-developers that it can be evolved into
something really neat in the foreseeable future.

Linux and fetchmail both went public with strong, attractive basic designs. Many people thinking about the
bazaar model as I have presented it have correctly considered this critical, then jumped from that to the
conclusion that a high degree of design intuition and cleverness in the project leader is indispensable.

But Linus got his design from Unix. I got mine initially from the ancestral popclient (though it would later


change a great deal, much more proportionately speaking than has Linux). So does the leader/coordinator
for a bazaar-style effort really have to have exceptional design talent, or can he get by through leveraging
the design talent of others?

I think it is not critical that the coordinator be able to originate designs of exceptional brilliance, but it is
absolutely critical that the coordinator be able to recognize good design ideas from others.

Both the Linux and fetchmail projects show evidence of this. Linus, while not (as previously discussed) a
spectacularly original designer, has displayed a powerful knack for recognizing good design and integrating
it into the Linux kernel. And I have already described how the single most powerful design idea in fetchmail
(SMTP forwarding) came from somebody else.

Early audiences of this essay complimented me by suggesting that I am prone to undervalue design
originality in bazaar projects because I have a lot of it myself, and therefore take it for granted. There may
be some truth to this; design (as opposed to coding or debugging) is certainly my strongest skill.

But the problem with being clever and original in software design is that it gets to be a habit—you start
reflexively making things cute and complicated when you should be keeping them robust and simple. I
have had projects crash on me because I made this mistake, but I managed to avoid this with fetchmail.

So I believe the fetchmail project succeeded partly because I restrained my tendency to be clever; this
argues (at least) against design originality being essential for successful bazaar projects. And consider Linux.
Suppose Linus Torvalds had been trying to pull off fundamental innovations in operating system design
during the development; does it seem at all likely that the resulting kernel would be as stable and successful
as what we have?

A certain base level of design and coding skill is required, of course, but I expect almost anybody seriously
thinking of launching a bazaar effort will already be above that minimum. The open-source community's
internal market in reputation exerts subtle pressure on people not to launch development efforts they're
not competent to follow through on. So far this seems to have worked pretty well.

There is another kind of skill not normally associated with software development which I think is as
important as design cleverness to bazaar projects—and it may be more important. A bazaar project
coordinator or leader must have good people and communications skills.

This should be obvious. In order to build a development community, you need to attract people, interest
them in what you're doing, and keep them happy about the amount of work they're doing. Technical sizzle
will go a long way towards accomplishing this, but it's far from the whole story. The personality you project
matters, too.

It is not a coincidence that Linus is a nice guy who makes people like him and want to help him. It's not a
coincidence that I'm an energetic extrovert who enjoys working a crowd and has some of the delivery and


instincts of a stand-up comic. To make the bazaar model work, it helps enormously if you have at least a
little skill at charming people.

## The Social Context of Open-Source Software

It is truly written: the best hacks start out as personal solutions to the author's everyday problems, and
spread because the problem turns out to be typical for a large class of users. This takes us back to the
matter of rule 1, restated in a perhaps more useful way:

18. To solve an interesting problem, start by finding a problem that is interesting to you.

So it was with Carl Harris and the ancestral popclient, and so with me and fetchmail. But this has been
understood for a long time. The interesting point, the point that the histories of Linux and fetchmail seem
to demand we focus on, is the next stage—the evolution of software in the presence of a large and active
community of users and co-developers.

In The Mythical Man-Month, Fred Brooks observed that programmer time is not fungible; adding developers
to a late software project makes it later. As we've seen previously, he argued that the complexity and
communication costs of a project rise with the square of the number of developers, while work done only
rises linearly. Brooks's Law has been widely regarded as a truism. But we've examined in this essay an
number of ways in which the process of open-source development falsifies the assumptionms behind it—
and, empirically, if Brooks's Law were the whole picture Linux would be impossible.

Gerald Weinberg's classic The Psychology of Computer Programming supplied what, in hindsight, we can
see as a vital correction to Brooks. In his discussion of ''egoless programming'', Weinberg observed that in
shops where developers are not territorial about their code, and encourage other people to look for bugs
and potential improvements in it, improvement happens dramatically faster than elsewhere. (Recently, Kent
Beck's 'extreme programming' technique of deploying coders in pairs looking over one anothers' shoulders
might be seen as an attempt to force this effect.)

Weinberg's choice of terminology has perhaps prevented his analysis from gaining the acceptance it
deserved—one has to smile at the thought of describing Internet hackers as ''egoless''. But I think his
argument looks more compelling today than ever.

The bazaar method, by harnessing the full power of the ''egoless programming'' effect, strongly mitigates
the effect of Brooks's Law. The principle behind Brooks's Law is not repealed, but given a large developer
population and cheap communications its effects can be swamped by competing nonlinearities that are not
otherwise visible. This resembles the relationship between Newtonian and Einsteinian physics—the older
system is still valid at low energies, but if you push mass and velocity high enough you get surprises like
nuclear explosions or Linux.

The history of Unix should have prepared us for what we're learning from Linux (and what I've verified


experimentally on a smaller scale by deliberately copying Linus's methods [EGCS]). That is, while coding
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
developers. Bell Labs, the MIT AI and LCS labs, UC Berkeley—these became the home of innovations that
are legendary and still potent.

Linux was the first project for which a conscious and successful effort to use the entire world as its talent
pool was made. I don't think it's a coincidence that the gestation period of Linux coincided with the birth
of the World Wide Web, and that Linux left its infancy during the same period in 1993–1994 that saw the
takeoff of the ISP industry and the explosion of mainstream interest in the Internet. Linus was the first
person who learned how to play by the new rules that pervasive Internet access made possible.

While cheap Internet was a necessary condition for the Linux model to evolve, I think it was not by itself a
sufficient condition. Another vital factor was the development of a leadership style and set of cooperative
customs that could allow developers to attract co-developers and get maximum leverage out of the medium.

But what is this leadership style and what are these customs? They cannot be based on power
relationships—and even if they could be, leadership by coercion would not produce the results we see.
Weinberg quotes the autobiography of the 19th-century Russian anarchist Pyotr Alexeyvich Kropotkin's
Memoirs of a Revolutionist to good effect on this subject:

Having been brought up in a serf -owner's family, I entered active life, like all young men of my time, with
a great deal of confidence in the necessity of commanding, ordering, scolding, punishing and the like. But
when, at an early stage, I had to manage serious enterprises and to deal with [free] men, and when each
mistake would lead at once to heavy consequences, I began to appreciate the difference between acting
on the principle of command and discipline and acting on the principle of common understanding. The
former works admirably in a military parade, but it is worth nothing where real life is concerned, and the
aim can be achieved only through the severe effort of many converging wills.

The ''severe effort of many converging wills'' is precisely what a project like Linux requires—and the
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

We may view Linus's method as a way to create an efficient market in ''egoboo''—to connect the selfishness
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


