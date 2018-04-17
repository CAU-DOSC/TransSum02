# 성당과 시장

필자는 가장 중요한 소프트웨어 (운영체제나 Emacs 같이 대단히 커다란 커다란 도구들) 은 성당을 건축하듯이, 즉 몇 명의 도사 프로그래머나 작은 그룹의 뛰어난 프로그래머들에 의해 조심스럽게 만들어지고 베타버전도 필요없이 발표되어야 한다고 생각했던 것이다.

필자는 리눅스에서 고요하고 신성한 성당의 건축방식은 찾아볼 수 없었다. 대신, 리눅스 공동체는 서로다른 의견과 접근방법이 난무하는 매우 소란스러운 시장같았고 이런 시장바닥에서 조리있고 안정적인 시스템이 나온다는 것은 연속된 기적으로만 가능한 것처럼 보였다. 필자는 시장 스타일이 매우 효과적이라는 사실은 분명히 충격이었고 이를 이해하려고 애썼다.

1996년 중반에야 이해가 되기 시작했다. 필자의 이론을 시험해 볼 수 있는 완벽한 기회가 오픈소스 프로젝트의 형태로 찾아왔다. 여기에서 의식적으로 시장 스타일을 시도해 볼 수 있었고, 큰 성공을 거두었다. 이 글의 나머지 부분에서는 그 프로젝트에 대해 이야기하고 효과적인 오픈소스 개발에 대한 격언들을 제시한다.

## 메일은 배달되어야만 한다.

필자는 즉시 배달되는 인터넷 이메일에 매우 익숙해졌다. 복잡한 이유로 인해 집의 컴퓨터와 CCIL 사이에 SLIP 연결을 하기가 힘들었다. 마침내 성공하고 나자, 주기적으로 locke 에 접속해 메일이 왔는지 체크해 보는 것이 매우 귀찮은 일이라는 것을 알게 되었다. 필자는 메일이 snark 로 배달되었을 때 바로 알 수 있고, 자신의 도구들을 가지고 메일을 다룰 수 있게 되는 것을 원했다. 이 과정에서 필요한 클라이언트들 중 필자의 갈증을 해소시켜주지 못했다. 여기에서 첫 번째 교훈을 얻을 수 있다.

1. 소프트웨어의 모든 좋은 작업은 개발자의 개인 가려움증을 긁어 냄으로써 시작된다.

2. 좋은 프로그래머는 어떤 프로그램을 만들어야 할 지 안다. 위대한 프로그래머는 어떤 프로그램을 다시 만들어야 할 지 (그리고 재사용해야 할 지) 안다.

3. 한 명을 던질 계획을 세우십시오. 어쨌든, 당신은 할 것입니다. "(프레드 브룩스, 신화 인 달, 11 장)

다른 말로 하자면, 첫 번째 해결책을 구현할 때까지도 진짜 문제가 무엇인지 이해하지 못하는 경우가 종종 있다는 것이다. 두 번째가 되어서야 어떻게 하는 것이 옳은 것인지 충분히 알게 될 수 있다. 따라서 만일 올바른 방법을 찾고 싶다면 최소한 한 번은 처음부터 다시 시작할 준비를 해 두어야 한다.

4. 적절한 태도를 가지고 있으면 흥미로운 문제가 당신을 찾아갈 것이다.

5. 프로그램에 흥미를 잃었다면 프로그램에 대한 당신의 마지막 의무는 능력있는 후임자에게 프로그램을 넘겨주는 것이다.

## 사용자가 있다는 것의 중요성

사용자들이 있다는 것은 매우 좋은 일이다. 당신이 누군가의 필요를 충족시켜주고 있으며 일을 잘 해나가고 있다는 것을 보여주기 때문만은 아니다. 적절하게 유도해 준다면 사용자들을 공동개발자가 될 수 있다.

6. 사용자를 공동 개발자로 취급하는 것은 신속한 코드의 개선과 효과적인 디버깅을위한 가장 간단하고 쉬운 방법이다.

이 효과의 위력은 과소평가되기 쉽다. 사실, 오픈소스의 세계의 우리들조차 시스템의 복잡도에 대항하여 많은 수의 사용자가 얼마나 힘이 되는지를 리누스 토발즈가 보여주기 전까지 과소평가하고 있었다. 

나는 리누스의 가장 중요한 해킹은 리눅스 개발모델을 만든 것이라고 생각한다. 리눅스의 성공과 방법론에 대한 선례는 GNU Emacs Lisp 라이브러리와 Lisp 코드 아카이브에서 찾아볼 수 있다. Lisp 코드 풀의 진화는 유동적이었고, 사용자가 주도한 것이었다. 아이디어와 프로토타입 모드들은 안정적인 최종형태를 갖추기까지 종종 3~4번씩 다시 쓰여졌다. 느슨하게 묶인 공동작업이 인터넷으로 인해 가능해 졌고, 리눅스 에서처럼 매우 자주 일어나는 일이 되었다.

## 일찍, 그리고 자주 발표하라

7. 일찍 배포하고 자주 배포하라. 그리고 사용자들에게 귀를 기울여라.
초기에 리누스는 하루에 한 번 이상 새로운 커널을 발표하기까지 했다. 리누스가 공동개발자들 이라는 자신의 기반을 잘 만들었고, 인터넷이라는 지렛대를 이용하여 누구보다도 열심히 협동작업에 몰두했기 때문에 이런 방식은 성공했다.
리누스는 그의 해커/사용자들에게 지속적인 자극과 보답을 제공했다 -- 리눅스 개발에 참여함으로써 자기만족을 얻으리라는 전망에 자극 받았고, 그들이 하는 일이 계속해서 (어떤 때는 날마다) 향상되고 있다는 것이 보답이 되었다. 

8. 충분히 많은 베타테스터와 공동개발자가 있으면 거의 모든 문제들은 빨리 파악될 것이고 쉽게 고치는 사람이 있게 마련이다. 
덜 형식적으로 말하자면, '보고 있는 눈이 충분히 많으면 찾지 못할 버그는 없다.' 나는 이것을 '리누스의 법칙' 이라고 부른다.
리누스는 문제를 이해하고 고치는 사람이 그 문제를 처음 파악한 사람과 항상 같은 것이 아니라 오히려 다른 경우가 더 많다고 이의를 제기했다 가장 중요한 점은 사람이 충분히 많을 경우 이 두 가지가 모두 매우 빨리 일어나는 경향이 있다는 것이다.
여기에 성당 건축과 시장 스타일의 핵심적인 차이점이 있다. 프로그래밍의 성당 건축방식 관점에서 보자면 버그와 개발 문제는 어렵고, 까다로우며 심오한 현상이다. 문제를 해결하려면 소수의 사람이 정밀한 검사를 수행해야 모두 끝났다는 확신을 가질 수 있다. 따라서 발표 사이의 기간이 길어지고, 오랫동안 기다린 릴리즈가 완벽하지 않을 때는 실망이 따른다. 반면, 시장의 관점에서는 최소한 새로운 릴리즈가 나올 때마다 그것과 씨름하는 수천의 열정적인 공동개발자들에게 알려진다면 금방 쉽게 해결할 수 있는 문제로 바뀐다. 따라서 더 많이 교정을 받고 싶다면 자주 발표해야 하며 덤으로 서투른 부분이 드러나더라도 잃을 것이 적다는 이점이 있다. 각 사람들이 버그를 찾아낼 때 조금씩 다른 개념의 집합과 분석 도구들을 사용하여 문제의 다른 각도에서 접근하기 때문이다. 
따라서 더 많은 베타테스터를 가지는 것은 개발자의 관점에서 현재의 '가장 심오한' 버그의 복잡성을 줄여주지는 않을 테지만, 누군가의 도구가 문제에 딱 들어맞아 그 버그가 그 사람에게는 쉽게 잡을 수 있는 것이 될 가능성을 높여준다.
얼마나 많은 눈이 복잡성를 다스리는가
시장 스타일이 디버깅과 코드 개발을 매우 가속화 한다는 것은 전체적으로 지켜봐야 할 점이다.
이해해야 할 한가지 중요한 점은 왜 소스에 대해서 모르는 사용자들이 보내는 버그 리포트가 유용하지 않다고 여겨지는지 파악하는 것이다. 소스를 모르는 사용자들은 증상의 표면만을 알려준다; 그들은 그들의 환경을 당연한 것으로 여기기 때문에 (a) 중요한 배경 데이터를 생략하고 (b) 좀처럼 버그를 재현하기 위한 믿을 만한 방안을 알려주지 않는다.
오픈소스 개발은 이 고리를 부수어 테스터와 개발자가 실제 소스코드를 기반으로 한 공유된 표현을 개발할 수 있도록 하고, 그것에 대해 효과적으로 소통할 수 있도록 하였다. 사실상, 외부에서 볼 수 있는 증상만 보고하는 버그 리포트의 종류와 개발자의 정신적 표현 프로그램에 직접 연결되는 종류 사이에는 개발자에 대한 영향력에 큰 차이가 있다.
이런 효과는 서로 다른 표면적인 증상들을 따라가 버그까지 가는 길이 증상을 보는 것 만으로 예측되지 않을 만큼 복잡할 경우에 더욱 증폭될 것이다. 이러한 경로를 순차적으로 추출하는 한 명의 개발자는 첫번째 시도에서 어려운 경로를 쉬운 경로라 생각하고 고를 가능성이 있다. 하지만 많은 사람이 빠른 배포를 하면서 경로를 병렬적으로 찾는다고 해보자. 그럼 그중 한 명이 가장 쉬운 경로를 찾을 것이고, 더 짧은 시간 안에 버그를 고칠 것이다. 프로젝트 지지자는 그것을 보고 새로운 릴리즈를 할 것이고 같은 버그를 고치던 다른 사람들은 더 어려운 방법을 시도해 보기 전에 멈출 수 있다.

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

당신은 나중에 결과가 필연적이고, 자연스럽고, 오히려 미리 정해진 것처럼 보일만큼 강력한 설계 아이디어로 그것에 빠져들어야 한다. 이와 같은 아이디어를 위해 노력할 수 있는 방법은 오직 수많은 아이디어를 가지고 있거나, 다른 사람들의 좋은 아이디어들을 창작자가 가야한다고 생각하는 방향으로 이끌 만큼의 공학적 정의를 가지고 있는 것 밖에 없다.
나는 칼 해리스와 해리 호크헤이서로부터 몇가지 아이디어를 얻었고 그것들을 열심히 발전시켰다. 우리 중 아무도 사람들이 천재적이라고 생각하는 낭만적인 방식으로 '독창적' 이지 못했다. 내가 이를 깨닫고 난 후에 쓰는 가장 첫번째이고 압도적으로 중요한 기능은 멀티드롭 서포트이다(multidrop support). 멀티드롭 서포트 기능을 추가하게 된 주된 이유는 이 기능이 나로 하여금 모든 경우에 대하여 주소 지정을 다루도록 하여, 싱글 드롭으로부터 버그를 해결할 수 있다고 생각하게 했기 때문이다.
다음은 멀티드롭 주소지정이 아주 뛰어난 설계 방안임을 밝혀준다:

14. 어떠한 도구도 예측가능한 방식으로나 절대 예측할 수 없는 방식으로나, 모두 유용해야 한다.

나의 베타 테스터에 의해 요구된 또다른 중요한 변화는 8비트 짜리의 MIME 연산을 지원하는 것이었다. 이 기능을 개발한 이유는 다음 규칙에 따른 것이다:

15. 어떤 종류의 게이트웨이 기반 소프트웨어를 설계할 때는, 데이터 스트림을 최대한 방해하지 않도록 노력하는 것이다!

## 인출 메일(fetchmail)로부터 몇가지 배울 점들

인출 메일 제어 문법의 언어 영역은 매우 제한되어 있기 때문에, 영어의 매우 작은 일부와 실제 제어 언어 사이에 정신적으로 이동하는 것의 혼란은 매우 적은 가능성이 있다. 이러한 것에 배울 점이 있다;

16. 당신의 언어가 Turing-complete 가까이에 없다면, 문법적 설탕은 당신의 친구가 될 것이다.
또다른 배울 점은 모호함에 의한 보안에 관한 것이다.

17. 보안 시스템은 자신의 비밀만큼이나 안전하다. 가짜 비밀을 조심하라.

## 시장(Bazaar) 스타일을 위한 필요한 전제조건

당신이 커뮤니티를 제작하기 시작했을 때, 당신이 제시할 수 있는 것은 '타당한 약속' 이다. 이는 (a) 프로그램이 잘은 아니더라도 돌아가는 것, 그리고 (b) 가능성이 있는 공동 개발자들에게 이것이 예측가능한 미래에 분명히 형태를 잘 갖춘 것으로 발전 할 수 있다고 설득하는 것이다.
나는 조정자가 다른 사람으로부터 좋은 설계 아이디어를 인지할 줄 아는 능력은 매우 중요하다고 생각한다. 리눅스와 인출 메일 프로젝트 모두 이러한 증거를 보여주고 있다. 예를 들어, 나는 내가 명석해 지려는 나의 경향을 억제했기 때문에, 인출 메일이 부분적으로 성공했다고 믿는다.
시장 프로젝트에 있어서 뛰어난 설계 능력만큼이나 중요한 기술이라고 생각되는 것은 시장 프로젝트의 조정자나 리더가 반드시 좋은 사람들과 의사소통 능력을 가지고 있어야 한다는 것이다.

## 오픈소스 소프트웨어에 대한 사회적 문맥

18. 흥미로운 문제를 해결하기 위해서는, 당신이 흥미를 느끼는 문제부터 찾아보기 시작하라

리눅스와 인출 메일의 역사가 우리로 하여금 집중하게 끔 하는 점은 사용자와 공동 개발자의 크고 활발한 커뮤니티의 존재 안에서의 소프트웨어의 발전이다.
"객관화 프로그래밍(egoless programming)"에 대한 제롤드 웨인버그의 주장에 따르면, 그는, 개발자들이 그들의 코드에 대해 텃세를 부리지 않고, 다른 사람들로 하여금 그 안에 있는 버그와 가능성 있는 향상을 보도록 격려하는 직장에서 프로그램 향상이 다른 어떤 곳에서 보다 매우 빠르게 나타난다고 주장한다.
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


