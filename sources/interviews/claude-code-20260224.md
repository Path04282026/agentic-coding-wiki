Welcome to the Vergecast, the flagship
podcast of pointing an LLM at just a
bunch of text files to [music] see what
happens. I'm your friend David Pierce
and I am sitting here getting ready for
the next season of Version History.
Version History, if you don't know, is
our tech rewatch show about the most
interesting good and bad products in
history. It's a very fun show. And for
this season, I have had to do research
that has taken me down rabbit holes
about Apple history, like deep into
Apple's history and into the history of
the monopoly that AT&T had for decades
over the phone business in the United
States. And that in particular is a
story I just frankly knew nothing about.
And I found myself reading a bunch of
tech history books, which is delightful.
First of all, I I I should read more
books. We should probably all read more
books. At this moment in time, my
information system is just insane. I'm
on social media. I'm scrolling through
apps. I'm on Reddit. I probably read
more words than I ever have. But it's
this like discombobulated
galaxy of just stuff all the time. And
to sit down and just open up a book and
stare at it for 3 hours has been like
genuinely cathartic in some really
interesting ways. So all of this is to
say go books is the official stance of
the Vergecast in 2026. But that's not
what we're here to talk about on this
episode. We're going to do two things on
this episode. We're going to talk
actually a bunch about AI. Um the first
thing we're going to do is talk to Boris
Churnney who created Claude Code at
Anthropic. Cloud Code came out a year
ago today, Tuesday, February 24th, as
you're hearing this and I think has kind
of become the single most important AI
product out there. So, we're going to
talk to Boris about where it came from,
what happened at the end of last year
that really made it take off, and where
all of this goes from here. I also have
a bunch of like product support
questions that I'm going to make him
answer because I can cuz he's coming on
the podcast. After that, the Virgin
Hayden Field is going to come on and
talk to us about how to think about your
own interactions with AI, particularly
as it pertains to data privacy and
security. We talked a bunch about this
stuff with OpenClaw and Moltbook a
couple of weeks ago, but I really want
to get into this idea of like if I'm
going to turn one of these things loose
on my computer to build software and
interact with my apps, how do I think
about that as a person in the world with
data and privacy and secrets? Reckoning
with that feels important. We're going
to talk about it. We also have a really
fun hotline question about gadget buying
in the year 2026 and why it's about to
be so complicated. All of that is coming
up in just a second, but I have a
chapter of this Macintosh book to
finish. Insanely Great by Steven Levy.
Highly recommend. And I have to go get
Claude Code to finish something before
Boris gets here. This is the Birdcast.
We'll be right back.
All right, we're back. So, for all
intents and purposes, we're about a year
into the Vibe Coding experience, and I
think vibe coding to me is the most
interesting piece of the AI equation
right now. I I'm continually skeptical
of the idea that chat bots are the
future of anything. Um I think there's a
lot of interesting technology in a lot
of these LLMs. I think agents are a cool
idea whose time has not yet come and
maybe never will. But the idea that you
can use AI to write good code is just
true. That thing has found product
market fit. And all of the external
questions about, you know, the the way
that these models are trained and the
energy that they consume, all of that is
real. But the idea that you can just
write code by prompting is is here and
it is real and it is powerful. Let me
just give you one example in my own
life. So I am constantly switching
productivity apps which means I have a
bunch of notes in like 10 different
apps. This is a terrible system because
I can never find anything but I I take
notes on meetings. I have like interview
transcripts. I have all kinds of stuff
just sort of scattered around. And over
the last couple of days, I've been using
cloud code to pull all of that data out
of all of these different apps, put it
all into one place in this app,
Obsidian, and then actually structure it
in a way that makes sense. So, I have
without any manual labor or moving stuff
around or messy copying and pasting on
my own, I have just been able to tell
Cloud Code, in this case, Co-work, which
is a version of Cloud Code, where my
stuff is and just have it go do the busy
work for me. That's powerful and
meaningful and a big deal and is a thing
that would have taken me a much much
longer amount of time to actually do and
that's just the tip of the iceberg of
what tools like Claude Code promise. So
Claude Code launched like I said earlier
a year ago today Tuesday as you're
hearing this and this felt like a good
moment for a variety of reasons to check
in on where we are with cloud code in
particular but also with this idea of
giving everyone the tools to write
software in general. So Boris Churnney
who created Claude code at anthropic uh
by accident is probably too strong but
certainly not imagining that it would
become what it has. He and I talked
about what vibe coding means, where it's
going to go from here, whether or not
there is a future of something like
cloud code that is actually useful and
usable for most people,
and how we're supposed to feel about the
end of people writing code at all. It
was a really interesting conversation. I
really enjoyed it. Learned a lot about
how to think about cloud code and other
things like it in my own life. I think
you'll enjoy it, too. Let's get into it.
Boris Journey, welcome to the Vergecast.
>> Yeah, thanks for having me.
>> You've you've talked a lot about kind of
the history of cloud code and and where
it came from and how you made it and now
that it's a year old. I think the thing
I'm particularly curious to talk about
is your relationship with coding now.
Um, one of the things I saw in all of
those interviews I've been watching is
everybody does the YouTube thing where
they like grab the splashy quote at the
beginning uh and and do it as sort of
the cold open and then they get into the
interview and over and over it's you
saying I don't write any code anymore.
Cloud code does 100% of my coding. And
this is like this this is a a a big
revoly statement to have made. Um, and I
want to get into what that actually
looks like. But over the course of the
last year or so as you've been building
it, have you undergone basically like a
complete reidentification of what it
means to be a coder and developer at
this point? It's surprising how little
of a change it's actually felt like as
someone that that writes code. I I think
part of it might be that in some ways
engineers are used to change because our
tech stack changes all the time. There's
always a new technology. There's always
a new framework, a new language. It's
just kind of part of the job is always
relearning and kind of re re- uh I don't
know it's like it's like every new every
few years there's a new stack and a new
language that's popular and so we're
just used to kind of you know figuring
it out and learning the latest thing. In
some ways it's felt like a big jump
because you know the big change over the
last year is I don't work with source
code anymore like I don't look at the
code of the program as much as I used
to. I I don't write any of it anymore
and that's been kind of a big change.
Back when we released quad code
originally in February, that was like
sonnet uh 3.5 new or 3. I forgot what
terrible name we gave that model. I
think it was 3.5 new. We should have
called it like 3.6 or something.
>> Yeah. AI model names not famously great
in the industry right now.
>> It's not not our strong suit. Not our
strong suit. [laughter]
Uh but so we released it and back then
you know quad code was writing maybe 10%
of my code. Uh when we released Sonnet 4
and Opus 4 in May that I think that
jumped to maybe like 30% or something.
It kind of creeped up over time, but
back in November when we launched Opus
4.5, that's when it it just suddenly
jumped for me from like 50% to 100%. And
that that was actually very sudden, but
it also just felt very natural.
>> What What does that change look like?
Like do you just wake up one day and
realize, oh, I'm not this thing has
stopped making mistakes. I don't need to
do it anymore.
>> Yeah. the as an engineer the way that
you would code maybe like I don't know
like middle of the year last year is you
kind of start the work in an agent and
an agent does the first pass but then
the code isn't isn't perfect there's a
bunch of stuff that doesn't work so then
I have to go in I have to test the code
I have to open it in a you know a text
editor to make some final changes to it
and what I realized around opus 4.5 is
one opus is now testing my code
>> so this was kind of cool like you know
it's like it's running the tests but
also So, it's able to open the browser
and it's able to kind of verify that,
you know, the website works correctly.
It can click around. If something's off
by a few pixels, it'll kind of move it
over and fix it. Uh, and then the second
thing is the code is just really good.
So, I I don't have to open a text editor
anymore. I I don't have to fiddle with
it by hand. And that was actually kind
of nice cuz that means I can move on to
the next thing and just write a little
bit more code a little faster. It really
does feel like that Claude moment sort
of happened overnight. It was like
everybody went home for the holidays,
got bored, used cloud code, went, "Oh my
god." and we were sort of off and
running. Um, but it seems like you as
the person who pays incredibly close
attention to it all the time also had
that big a kind of overnight shift in
how you think about it. Was it just
big new model all of a sudden had this
new capability that no one was expecting
it to do this well? Like what what
accounts for that big a change that
quickly?
for the robotic for the longest time
coding has been a thing that we just
want the model to be really good at
because you know essentially the road to
safe AGI like this model is going to be
very it's going to be intelligent at
some point it's going to be super
intelligent our job at anthropic is to
make sure that goes well and that it's
done in a safe way um so the model
doesn't do bad stuff and so you know
this is kind of aligned with the
interests of what the users want and of
humanity broadly and the model is is
software and the way that it interacts
with the world is through tools and
through other software that it writes.
And so for us, for the longest time,
we've had this belief that the way to
safe AGI is through coding and then kind
of tool use and then uh computer use. So
this kind of increasing capabilities to
interact with the world, but it's always
mediated through code. So it always goes
through code. When you do model
training, you try a lot of stuff.
There's a lot of experiments. There's a
lot of new ideas that people are trying
all the time. A lot of times it just
doesn't work. Um but sometimes it does.
And you know, for Opus 4.5, the
direction was kind of set early on
because we knew where we want to be
headed. But, uh, it just turned out that
a bunch of good ideas worked and, um,
there was just a big step change. It was
just as surprising for me as it was for
everyone else. One of the things I have
been trying to figure out and one of the
things we've talked a lot about on this
show and at the Virgin General is
ultimately
who the end user of something like Cloud
Code is. And I think right now it's
fairly clear, right? especially for a
for a product in the terminal. It is a
it is a developer product for
developers. Is that fair to say right
now?
>> We designed it as a developer product
for developers but even from the
earliest days all sorts of
non-developers started using it and this
this was just the craziest surprise but
also you know the best possible thing
that you can see in product is people
people want to use it so much they jump
through hoops uh to to use it.
>> Yeah, that is definitely a thing we've
seen with a lot of these tools. I mean I
I was playing around with some like open
claw and some of the stuff like that and
the the amount of work you have to do as
a just normal lay person to get some of
these things up and running is pretty
remarkable and yet people are are
willing to do it. I suspect there are a
lot of people who had never heard of
their terminal until cloud code started
to happen in their lives.
>> Yeah.
>> Yeah. That's right. And and you know
like now all you know all the biggest
companies in the world use quad code.
It's like Spotify, Shopify, like Vamp,
Netflix, Nova, Nordis, like Nvidia,
Snowflake, Salesfor everyone uses quad
code. The small startups use it. But
also a thing that we're starting to hear
is even at these bigger companies, a lot
of people that are not engineers are
using quad code. And so I think like
ramp just tweeted about this pretty
recently that they have a bunch of
product managers, data scientists, a lot
of people using it. So even at these
biggest companies, this is kind of what
we're seeing. And this was also by the
way like the reason we launched co-work
is we see people using quad code for
things that are not coding and we're
like all right I think we can do better
than a terminal for you and so we build
a thing that we think they would
actually want to use and this is a thing
we're still learning about and we're
seeing how people actually use it.
>> Yeah. So I I I am talk to me about that
early signal a little bit when you start
to see people who are not developers who
are not traditionally people who would
be in an ID and thinking about code and
thinking about the terminal start to use
this product. I remember walking into
the office and Brandon who's our data
scientist was using quad code in a
terminal to do data analysis and he had
like little charts in the terminal and
stuff and I was like this is just crazy
like there's no way this is the best way
to do it [laughter] and he he was like
no it's great and uh the next day he had
like three plot codes running at the
same time doing like data analysis in
parallel and uh then all the data
scientists started using it but I
actually still didn't really get it cuz
I thought there's something weird about
you know maybe people that work at
anthropic maybe they're very early
adopters more willing to try these new
tools cuz you know it's like engineers
are always the early adopters and you
know they try a thing and then
eventually everyone else tries the
thing.
>> Uh but I think by the time that I think
now like half of our sales team uses
quad code every week.
>> Uh I think when that started happening
that's when I really started to get it
that uh this is a product that's not
just for engineers and we got to make
that easier.
>> So yeah I would think that realization
would lead you in one of two directions.
One is to say, okay, actually
we're giving people access to a
developer tool and maybe we should do it
in developery ways, right? That maybe
have people understanding what the
terminal is on their computer is not the
worst thing in the world. And if people
are willing to go through these hoops to
do this thing, maybe we're on to
something. Maybe we don't need to sort
of radically rethink the UI because
people are figuring it out. or you look
at that and say we need to radically
rethink the UI because these people are
having to jump through these crazy hoops
just to do the work that they want to
do. Uh do you have a stance on which one
of those is the right reaction?
>> Yeah. So I mean look like we we we
started in a terminal but pretty quickly
we started experimenting with other form
factors too. So you know we have like ID
extensions for like VS Code cursor Jet
Brains IDs. There's
>> we have like iOS and Android apps. I
actually do like probably a third of my
code on the iOS app nowadays.
>> Really? I never would have predicted
that but you know that's that's where we
are. We have like a web surface there
there's a desktop app so like you know
the the same desktop app that has
co-work also has quad code in it so you
know you can use the exact same quad
code so we're just like always
experimenting with this but yeah it's
like the the surface is just a little
bit different for different kinds of
users.
>> Yeah.
>> So co-work under the hood is just quad
code. It's like the same agent SDK. It's
the you know the the it's it's an
awesome agent and it's the same exact
one that's that's running everywhere.
But for people that aren't engineers, we
want it to be a little less foot gunny.
Like we don't want people to mess up
their system and things like this. So we
actually ship like a whole virtual
machine. We have deletion protection
built in. There's a whole bunch of
things that we built for less technical
users that engineers would actually find
kind of annoying and they wouldn't want
in the way versus for engineers, there's
something a little bit different about
the tool because engineers love to
customize everything. If you talk to
like two engineers, they're going to use
their tools totally differently. There's
no two engineers that have the same
setup. And so the way that we build quad
code across every surface across
terminal, IDE, desktop, everything is we
want it to be the single most
customizable dev tool that anyone has
used.
>> So it's very very config configurable.
You can hold it however you want. You
can customize it however you want.
There's hundreds of ways to configure
it. And what's also kind of cool is
because quad code is quad code, you can
just ask quad code to configure it for
for you. So you can just be like you
know change the theme or um you know
like change the setting or change the
setting. It can just do that for go.
>> See this is one of the things I have
really enjoyed about my own experience
with cloud code is there's so much of it
that is sort of relearning what's
possible in certain ways. Like the idea
of asking cloud code to reskin itself
because I don't like the color scheme.
It had just never occurred to me. I
don't like the color scheme and I would
like a different one, but it literally
had just never occurred to me to ask
this thing that is writing code for me
to write that bit of code for me. And I
feel like this is kind of why I'm
curious about your own relationship with
writing code is it just it yes, there
are certain things you have to do, but I
feel like I don't know I would think of
learning a new coding language as like
learning how to play a new kind of
instrument, right? Where it's so a lot
of the behavior is the same just pointed
in new directions with new details and
new systems to to figure out. But this
is like, you know, you used to play the
violin and now you're on the soccer
team. It's just like it's a completely
different way of thinking about how to
use your body. Do you know what I mean?
>> Yeah. Yeah. Yeah. The way the way I
would think about it is like you used to
play the violin and now you're like
you're conducting the orchestra.
>> Okay.
>> That's like
>> that's a good way to think about it.
>> But it's also Yeah. I mean the hardest
thing for me is just changing
expectations every time a model comes
out. It's just so quick,
>> you know, like this thing that just
never would have worked for Sonnet 4,
Sonnet 3.7, now with Sonnet 4.6, it just
works
>> and I just have to constantly rearn
this. All the stuff that I would have
thought, you know, didn't work. Um, I
just assume it'll work at some point.
>> Do you do you have like a list somewhere
of all the things that are broken that
you try every time a new model comes out
and just check some things off the list?
>> Essentially anything that I do by hand.
>> Oh, interesting. Okay.
>> Yeah. Yeah. So, so for example like
sonnet 4 opus 4 they were and and even
like 4.5 it was okay at this but four
was like not great at it if we have like
a feedback group we have this like slack
channel where all the anthropic
employees get feedback about quad code
we also have a lot of external feedback
channels for customers and github and
things like this and before the model is
not very good at looking at the feedback
channel and deciding what to do and what
to fix but now actually a lot of the
code that we ship for cloud code you
know like cloud code is 100% written by
cloud code at this point but also I
would say maybe 20 30% of that is quad
code just looking at the feedback group
figuring out the kinds of things people
are reporting and then automatically
fixing it and this kind of proactivity
just would not have been possible with
older models but with like with opus 4.5
opus 4.6 it actually just started
working.
>> Yeah. What have you learned with co-work
in particular because I I would assume
like you said there's a different set of
person coming to co-work than to cloud
code with a with a different set of
expectations and a different set of
knowledge. are are they using it kind of
radically differently and and making you
rethink this whole system all over
again?
>> You know the the most surprising thing
so at this point cloud code there was
some study that it writes like 4% of all
the commits in the world of you know
like all the all the code in the in the
world I think the number is actually
quite a bit higher than that cuz it's
not including private code and also our
growth has inlected since that study
it's actually going up even faster than
before. So I think it's actually quite a
bit higher.
>> In the early days though clot code did
not grow very fast. Um it was not a hit
originally. It took like a few months to
catch on because it was just such a new
idea.
>> Um co-work on the other hand has been a
hit immediately. So like as soon as we
launched it, it's just been, you know,
exponential since. And this is what we
like to see cuz we also we think in
exponentials. So I think the biggest
thing that's been surprising is just how
quickly it's been growing, how quickly
people have figured out how to use this.
>> Why do you think that is? What do you
feel like you got right about co-work?
>> There was just a pent up demand. I think
that was like the biggest thing like
just for something a little more
understandable.
>> Yeah. More understandable. Like you saw
all these people on Twitter that are
using like quad code to they were like
growing tomato plants like recovering
corrupted photos off of like off of a
hard drive. Like someone like used to
recover wedding photos. Um Pietro I
think who actually used to work at
Anthropic used it to um I think it was
like genome analysis like he got his
like genome sequence and then he like
quadcode to look at like you know like
specific sequences and stuff. um quad
code not intended for medical advice but
[laughter] he did use it for the there's
someone that using it for like for MRIs.
So I think just this like this pent-up
demand is the single greatest thing that
you can see in product cuz it just means
like people are knocking down the door
and they're jumping through hoops for
you know this this this terminal thing
that wasn't really designed for this.
>> So it it was pretty obvious I think that
it it would have been a hit. One of the
most interesting things about co-work in
particular to me has been that the
product itself is really focused on uh
sort of busy work I guess is the way I
would put it. Like it's you open it up
and one of the first things it offers is
to organize your screenshots, right?
Where it's not
>> it's not build a a dashboard of your
entire life. Like one of the jokes we
always make on the show is that
everybody looks at AI tools and the
first thing they say is I want to build
a daily planner because all of my
information is ever this is like the
first idea everybody has about what to
build with AI. It's just a thing to tell
me what matters in my life. Um but I
think the the real truth of like
software forever is that this thing this
stuff all starts by just sort of solving
relatively straightforward relatively
simple problems for people. like I need
to do math and so spreadsheets exist,
right? Like this is what it is. And I
think to me one of the most eye opening
things about co-work was it just has a
bunch of ideas of little things it can
do for me that would take me a long time
to do on my own that aren't hard.
They're just this is a tool that will
automate away a bunch of my busy work on
my computer. And and my sense is that is
the kind of thing that has just every
single person in the world resonates
with the idea of that in a way that
strikes me as very powerful. And it's
not as open-ended as you can build any
kind of software you can imagine. Uh or
you can talk to this chatbot about
anything. It's organize your
screenshots. And I think that is like a
surprisingly powerful bit of of product
to put in there like that.
>> Yeah, absolutely. And if you want to
build something, you just, you know, hit
the code tab in the desktop app and you
can go build whatever. But
>> if you want to if you want to organize
your desktop, like I actually I use code
for a lot of stuff. Like I used it to
pay a parking ticket the other day. Um I
was up in Seattle. We went clamming and
I used it to purchase a clamming
license. So that was pretty awesome.
Like I just did something else and it
navigated this like actually kind of
annoying government website to do it.
Someone on the team is using it to pay
their taxes right now. Um, so also not
financial advice, but uh it's actually
like quite useful for all all this
different kinds of stuff. This is one of
the things that's like also kind of hard
to explain to people is like people ask
what do I use it for? And my answer is
well kind of everything. It's like all
all the toil like all the stuff you
didn't want to do by hand it can just do
so you can do the stuff you actually
want to do.
>> Yeah. So, I I think okay, let's talk
about doing taxes, which I know is is
just an example off the top of your
head, but I think is is a useful
sort of middle ground of the kinds of
stuff that I think about a lot with AI
where organize my screenshots is
relatively low risk, right? Like the the
idea of it might delete a thing that I
didn't want it to delete, but in
general, it's just going to put things
in places and delete stuff off of my
computer that I don't want. And I think
you can get people comfortable with
doing things like that on their
computers fairly quickly. Um,
have co-work do my taxes just has
naturally more consequences, right? And
and I think
>> part of I know a question you get asked
a lot and also a thing that I think is
tricky with a lot of these tools is it's
one thing to have it write code that I
can then go check even if I don't,
right? the responsibility is back on me
to check it and make sure that I
understand what it is and and code is
legible to me as a developer. But if I'm
just a person and I'm like, "Cowork, go
do my taxes for me." How much faith is
it reasonable or fair or rational to
have in co-work or cloud code or any
tool to go just execute that entirely on
my behalf? At this point,
>> the tools are not perfect and it's still
early, but they are surprisingly good at
things that people often expect they
would not be good at. And again, it just
improves with every model. For something
like taxes, I would definitely I'll
definitely double check it. So, like
have do the tax and and actually the
thing that that I would do is say do the
taxes, but then triple check your
results and just have cowork do that
work for you and and then by the time
you check it, there's a very high chance
it's just going to be pretty good.
>> Yeah. Actually, the to your point about
you you can have it test itself that
actually I think that there's there's
something very powerful about that too.
But part of the reason I bring up taxes
is because the the last innovation in
tax software was that it will scan your
W2s for you. Um I don't this is this I
remember this being a very big deal in
my life where I didn't have to type out
that I could just upload the PDF of my
W2 and it would just pull in all the
information. And I remember for a minute
it was like okay you have to check that
because the the scanners the scanning
system isn't perfect. The software won't
get it exactly right. But now like I
don't remember the last time I double
checked the numbers. I just you just you
just upload the W2, it shows up in the
field and you move on with your life.
And I I wonder it feels like we are just
barreling towards that with with all of
these tools too that it's like
>> there's going to be a beat of I mean I
guess it's like your experience with
cloud code. There's going to be a beat
of I need to check its work and then a
beat of well I'll spot check it and then
we get to I'm just not worried about it
anymore. And that's like that's the
right end state. It just it doesn't feel
like we're quite there yet.
>> Right. Right. I mean it's like two
things that happen at the same time.
It's like the model gets better and the
product gets better and then also as
like as users we get more comfortable
with this thing and like both things
kind of happen at the same time. Before
before we released coowork I was using
it to do all of our project management
for the team when I was like first
testing it out and I still actually use
it for this like every week. So we have
a spreadsheet of kind of all the things
the team is working on and we ask the
team to just like fill out their status
every week. So just say like is it on
track, is it off track? And so I just
have co-work like ping people on Slack
if uh they haven't filled it out. And so
all I do is I'm like, "Hey co-work, open
the spreadsheet." And then for anyone
that hasn't filled it out, message them
on Slack. It'll just do it perfectly. Um
there's actually one person's name that
it for some reason can't figure out on
on Slack. So I have to do that. But
otherwise, it just does it. And I was
actually like kind of taken it back cuz
I didn't even realize that it would be
able to do this. So, I would just like
experiment with this. Double check until
you're comfortable, but I think we'll be
there pretty soon.
>> In a case like that, does it message
people on Slack as you or as like a a
bot?
>> I asked it to sign its messages as
co-work.
Okay.
>> Yeah. And co-work actually know it
supports this thing. Uh in quad code, we
have this idea called claude.md. It's
just like a special file, but
essentially it's like all the
instructions you want claude to take
into account every time. Uh so coowork
also supports this now. So you can just
say like whenever you message people on
Slack, sign yourself as you know send
sign them message as like co-orker bot
or something. It'll just do that.
>> Yeah, that's smart. Yeah, I think that
that kind of there's a little bit of
transparency there that I think it's
interesting. Like I remember you said in
one interview I was I was watching that
uh co-work occasionally in the course of
doing stuff for you go and tweet on your
behalf and that that always felt kind of
strange.
>> Yeah. Yeah. Yeah. U it's funny actually.
Quad code does this too pretty
consistently now. when when I'm like
debugging something sometimes quad will
be like hey this code is kind of weird
let me like look at the history so it'll
look at the history of the code in git
once in a while it sees a really weird
change by someone and it'll message that
engineer on swack just to get context
it'll wait on the response and then I've
also seen it push back so like the
engineer is like yeah I did this change
for this reason and then and then quad
code is like well I don't think that's a
very good reason and I think you
actually introduced a bug so let me go
ahead and fix that [laughter]
how are you thinking about the rest of
the UI around this stuff. I think like
so much of Claude code and co-work are
are very chatbased still. Uh does that
feel like the right UI to you going
forward or is there more work to do
there?
>> We are constantly experimenting with new
ideas. I think the UI of the future has
not been discovered yet. So we have a
lot of experiments in flight. I would
expect it to change there. There's going
to be a lot of things that we test. The
single most important thing is just
seeing what people want. And so, like,
you know, I'm on Twitter and threads all
day and so is a lot of the team. We just
love talking to people. We love getting
the feedback because, you know, we have
a lot of ideas, but the only way to
figure out what the right ideas are are
to see what people say and to see what
people enjoy.
>> I agree with you that the UI of the
future has not been discovered yet. Do
you have a hypothesis at this moment in
early 2026 about what it might be?
>> I don't yet. I don't think we found it
to be honest. I think there's a lot of
ideas around like proactivity uh and uh
Claude kind of jumping in when it knows
that you're going to need help, but it's
kind of hard to get this boundary right.
Um cuz you don't want to end up with
something like Flippy. [laughter]
And it it speaks to I think the the
progression you're talking about a
little bit a from you know playing the
violin to conducting the orchestra. It's
just a different set of tools that are
available to you when when that's what
you're thinking about. But also you you
mentioned going from uh basically code
to tool use to computer use. Um can you
just walk me through what that
progression looks like as we go through
because I think we we've heard a lot
about uh agents to the point where I
think the word agent essent essentially
means nothing. Um agent is just like
magic that happens on your computer and
it's like sure whatever. Um, but I I
think you're you're thinking about this
in a in a much more sort of practical
how do we give this thing more powers
kind of way. Why does it go code tool
use computer use?
>> Yeah. Oh my god. Don't don't get me
started. Like the word Okay, I will get
started. The word agent I feel like
[laughter]
>> I feel like everyone just misuses it.
Like it has a really specific meaning
when you talk about like AI research,
when you talk about engineering. So an
agent is an LLM that you talk to, but
the LLM can use tools. This is the thing
that makes it an agent. It's like it can
use tools. Um, and so if you think about
uh without tool use, the agent can write
code. So let's say you give it a prompt
and it can kind of write some HTML or
something. And then as a user, you take
this and you kind of copy and paste it
into like a ID or something like this.
So this is just like the coding
capability. And as the model gets
smarter, it gets better and better at
working with big code bases. But there's
still kind of this problem that you hit
where at some point you just can't give
it all the context it needs. But you
know the model actually does know the
context that it needs because it's able
to search around and it's able to look
throughout the entire codebase. It's
able to look at Slack. It's able to look
at like the history of the code. Um it's
able to do all of this but it's just too
much information. Like you wouldn't be
able to give it all the information up
front. And so the answer is tools. Uh
you give the model tools and it can use
a tool to look at the code. It can it
can pull in more files. It can look at
history. It can do all this stuff. And
so this is why tool use is important.
It's you know the same as a person if
you don't have tools like you you
actually can't do a lot like just with
like just with your hands, right? You
need like keyboards, you need shovels,
you need like if you're cooking in the
kitchen, you need a whisk. Like these
things are just very uh there's not a
lot you can do without it. So it's kind
of the same thing for a model. And then
when you think about computer use,
there's just like a lot of things that
are kind of hard to interact with just
with tools. M
>> so if you think about like what what can
you actually do with a tool on a
computer it's something like MCP or it's
an API or it's a command line interface
but not everything has that so you know
like if you have like a I don't know
like this this like clamming thing I was
getting this like clamming license and
you know there's no API for that but
there's a website
>> and to use the website you want the
model to be able to use a browser you
want it to be able to use a computer um
and so this is kind of this natural
evolution so you start with coding uh
then you move on to tools and this is
the way to for to interact with the
world and so you don't have to spoon
feed the model context. It can just use
the tools to pull in context and then
computers are kind of the last the last
thing cuz then the model can just use
everything.
>> Okay. Do you think as AI continues to
grow and if it if it sort of takes over
all of software and computing the way
that that a lot of people think it's
going to that the computer use part
eventually becomes sort of obviated?
Like if there were enough tools and
enough MCP access and enough of the the
stuff that you're talking about,
does computer use just sort of an
elegant hack that gets around the stuff
that maybe will exist later and we won't
need it? Early on, in the early days of
using the model for coding, people were
talking about designing special
programming languages to make it so the
model can code better, right? And I
always thought this was kind of silly
because the model can just figure it
out, you know? It's it's not like us
where, you know, there's like a
programmer that likes Python, there's
another one that likes JavaScript and
like won't touch Python. The model's not
like that. It can just write whatever
language. It doesn't care. So, I think
it's kind of the same thing here. I
think over time, the model doesn't care.
whatever tools you give it, it will be
able to figure it out and it can use
those tools to do, you know, things for
you.
>> Talk to me about how how people should
think about kind of their own risk
profile in giving access to their data
and their computer and their files and
their photos and whatever to a system
like Cloud Code or or Cowwork. I think
you you have, you know, lots of
incentive to tell me that it's it's
totally fine. You can have all this
stuff on my computers. We're putting the
safeguards in. But how should people
think about what it means to give cloud
code access to a folder on my computer
even something like that?
>> Yeah, totally. So I would think about it
on a few levels. So the most basic level
is like why does anthropic exist? We
exist to make safe AGI. Um we you know
initially we have a bunch of founders
that you know like left a different AI
lab and came and started anthropic
>> ones that people have heard of. Yeah,
I'm I'm familiar. [laughter]
>> But this the reason we exist. Um and
there's a lot of coralers to safety. Um,
you know, like the security is actually
very important if you want to get safety
right. Uh, privacy is very important if
you want to get safety right. Um, and
uh, all of this stuff we sort of have to
do. We're very lucky that we care about
safety and so does our most important
target customer which is enterprise and
uh, companies. You know, there's a lot
of like consumers that use anthropic
products. This is awesome and this is
something we love to see. We will build
for you. But actually like the main
market we care about is enterprise as a
company
>> and we're very lucky and we picked this
market on purpose because we know
enterprises care a ton about safety and
security and privacy and so we built for
them and so like if you look at the
product it's actually kind of annoying
for me because like if someone has like
a quad code bug report or something I
literally cannot see your data um so
like I need you to like give me
reproduction stuff so I can like
reproduce it but I literally can't
access the data to to um you know like
see this issue. So there's a lot of like
controls like that in place also because
we care about safety a lot. There's a
lot of work that goes into just making
the model inherently more aligned and
interpretable. Um and this is also just
it it's very important and also very
related to this. And yeah, I mean the
final thing is like there's just a lot
of stuff that we build into the product
like co-work can only see the folders
that you give it access to. It cannot
see anything else on your computer. We
we we put an entire virtual machine in
co-work to make sure it's a really hard
security boundary uh to you know so it
can't access stuff that you don't give
it access to. The biggest thing to worry
about is attacks like uh prompt
injection anything like this that would
kind of exfiltrate your data.
>> Um we have a lot of protections in place
for this
>> and Opus 4.6 is just the most aligned
model that we've ever built for prompt
injection in particular and there's also
a lot of like runtime classifiers and
kind of safeguard that we put in place
for this but this is the biggest thing
that I will think about is as you have
co-work as you have cloud code interact
with the internet just be thoughtful
about what websites it is it's using and
it will ask you for permission but it's
a thing to keep an eye on because this
is not a solved problem yet it's quite
good but it's not yet solved
>> that's a good one um all right give me
one
like normal human co-work activity that
lots of people should do that you you've
either done or building or have heard
from people that not everybody might
expect that they should go do and then
I'm going to let you go.
>> Oh, a normal human. Uh, okay. One is
just like responding to email.
>> Just like open my Gmail, look at the top
three things I should respond to, draft
responses. So, you can do that quite
well. Um, a second one that I do is just
like canceling subscriptions. Um, so I
actually use it to cancel like
>> I canceled like a TV thing that I wasn't
watching.
>> That's the the most unbelievably
annoying thing to do. I'm going to make
cloud code unsubscribe to all of my
email newsletters that I don't want
anymore. Uh, [laughter] this is this is
going to work for me.
>> Yeah. Yeah. I I love this like dual
track. Like you can use it for your
write the emails and also unsubscribe
for email.
>> Yeah, [laughter] exactly. I just never
want to look at my email ever again. If
if Claude can make that happen, we will
have accomplished something.
>> AG. All right, Boris. Thank you so much.
I really appreciate you doing this.
>> Yeah. Yeah. Thanks, David.
>> We'll be right back. [music]
>> Support for the show comes from Built
Rewards. Unfortunately, no one will pat
you on the back for paying your rent on
time every month. But you can earn
rewards on your rent with Built, which
is like a pat on the back, but for your
wallet. [music] Built is the loyalty
program for renters and rewards you
monthly with points and exclusive
benefits in your neighborhood. With
Built, every time you pay your rent, you
earn points that you can put towards
flights, hotels, [music]
lift rides, Amazon.com purchases, and so
much more. And now, it's not just for
renters. Built members can earn points
on mortgage payments, too. Being a Built
member also unlocks exclusive benefits
with more than 45,000 restaurants,
fitness studios,armacies,
and other neighborhood partners. Join
the loyalty program for renters at
joinbuilt.com/verge.
That's j o i n b i l t.com/verge.
Make sure to use our URL so they know we
sent you.
Support for the show comes from Shopify.
Starting a new business, it could be a
lonely endeavor, especially in the
beginning. And if you're just starting
out, it's more important than ever to
make sure you have the right tools at
hand. If your business includes
e-commerce, a great next step is to try
Shopify. Shopify is the commerce
platform that millions of businesses
around the world rely on to sell their
products online. You can get started
with your own design studio with
hundreds of readytouse templates.
Shopify helps you build a beautiful
online store that matches your brand's
style. If you're asking yourself, "What
if people haven't heard about my brand?"
Shopify helps you find your customers
with easy to run email and social media
campaigns. And if you get stuck, Shopify
is always around to share advice with
their award-winning 24/7 customer
support. It's time to turn those whatifs
into. With Shopify today, you can sign
up for your $1 per month trial and start
selling today at shopify.com/vercast.
Go to shopify.com/vercast.
That's shopify.com/vercast.
Support for the show comes from Upwork.
When you run your own business, the
to-do list can feel endless. Well, that
might be the sign that it's time to grow
your team. And for that, there's Upwork.
Thousands of growing businesses already
trust Upwork to hire flexible,
highquality freelance talent for
everything from one-off projects to
ongoing support. Upwork also cuts down
operational hassle by handling things
like contracts and payments in one
place, so you can spend more time
running your business. Upwork is free to
use, but if you decide to upgrade to
Upwork Business Plus, you'll get access
to the [music] top 1% of talent on
Upwork. And with AI powered
shortlisting, you'll get matched to the
right freelancer in under 6 hours. You
can visit upwork.com
right now and post your job for free.
That's upwork.com to connect with top
talent ready to help your business grow.
That's up wk.com.
upwork.com.
All right, we're back. Hayden Field,
Verge, senior AI reporter is here. Hi,
Hayden.
>> Hi. Yeah.
>> So, you were here recently and we were
talking about Moltbook and Open Claw and
and all of the insane things that you
can do on your computer with AI tools
and AI agents. Um, and we talked a
little bit about privacy and kind of how
to think about whether or not you should
engage with these tools and install them
and what kind of data you should give to
them. And, um, I've realized I've been
having kind of varying levels of
existential crises about AI tools. Um,
starting with like I had a real
experience with OpenClaw where I I
downloaded the installer for OpenClaw
onto my computer. I have a I have a Mac
Mini and I have a MacBook Air and I was
on the MacBook Air and I downloaded
OpenClaw and I was like, I'm going to
use this, get into it, see what it's
like, try the whole thing out. Um, and I
got literally halfway through the
install process and was like, this is so
stupid. Like this computer is full of
all the information I care about in the
world and all of the stuff that I know
about everyone that I know, including
like important confidential information
as a journalist. Giving this unknowable
AI agent access to this is insane. So
that's one level. But then even like I
use I mostly use Claude for AI stuff and
one thing Claude really wants you to do
is connect your Gmail and connect your
Google calendar. And I've had moments of
being like is this an irresponsible
thing to do? Like am I being stupid
giving Claude access to my email? So,
what I want to do as best we can is just
try to think through sort of a how to
think about your data and AI framework.
Does that seem reasonable?
>> Perfect. I've been asking the same
questions.
>> Okay. So, let's just start kind of big
picture. I want to get into some sort of
nitty-gritty. I literally want you to
tell me if I should give Claude access
to my Gmail, but we'll get to that. Um,
you've been reporting on this a lot and
talking to experts and trying to think
through this for yourself. Do you have
kind of big picture guidance on just how
people should be thinking about this
stuff?
>> Yes. And this is perfect because the big
picture is the easiest to get at, you
know, because it's it's really a
different it's a different decision for
each person depending on their risk
tolerance and like the other ways they
live their life. So honestly, the big
picture is the easiest to kind of square
and then everyone can kind of make the
rules for themselves. But I did a bunch
of expert interviews this week just to
make sure my instincts are kind of on
track with what actual privacy experts
and you know tech leaders are thinking
and it seemed like they were in that
basically it's hard to give people good
advice on this stuff that stays current
over time. That's what some of the
privacy experts I was talking to said.
You know it's like every six months
things could change every month every
year. So, you know, the as of this
moment in time, a lot of people are
essentially kind of ignoring the way
that they usually, you know, evaluate
their risk tolerance and just kind of
adopting AI tools that are going viral
or being talked about a lot just because
of FOMO or like, you know, the promise
of making your life a lot easier. We all
as humans want to make our lives easier.
Um, you know, one expert I talked to
said it was like the siren song or like
teenager mode. It's like, you know,
you're just you want to have the
short-term gain and make your life
easier. You don't really want to think
about the long-term stuff sometimes.
That's fine. But
>> teenager mode is such a good way to
think about it.
>> Yeah, [laughter] that was not quite full
like yolo mode like lol nothing matters,
but it's it's a little bit like my brain
is just not yet fully developed,
[laughter] you know?
>> An executive dark told me that it was
like, you know, doing your seat belt
versus just being like, ah, it's fine.
I'll just drive [laughter] past the
highway. So, yeah, exactly. It's like,
you know, basically you need to treat AI
tools the exact same as you would treat
any other service that was requesting a
lot of data from you and maybe even with
a sharper eye because these companies
are newer, they're less time-t tested
and they're also more incentivized to
move quickly and they have a little bit
less uh, you know, regulatory frameworks
on them. So, you know, a lot of times
they're voluntarily complying with
certain rules. Um, you know, and that's
all well and good, but you know,
something that a bunch of experts told
me is, yeah, they can change that at any
time. You know, they can kind of on the
DL like shift that voluntary framework,
shift that little like our mission um
any time, you know, and there's no
hammer coming down on them if they do uh
they are just it's voluntary. So,
they're doing it as a favor. And you
know that means that they can shift at
any any given time and change how they
treat your data, who they share your
data with, how they use your data to
train their own systems or not. Um, and
the other thing is these companies may
get bought eventually. You know, one
expert I spoke with was like if you
wouldn't feel comfortable with your
employer um knowing certain things about
you uh 5 years from now when Opening Eye
gets sold and you know, they're selling
off the info to the highest bidder.
Again, that was like a extreme scenario,
but he was like, "Yeah, don't share it."
So, you know, that's something to keep
in mind, too, with like sharing health
stuff. Like, do you want insurance
companies to find out certain things
about you and change your premium?
Again, that's an extreme scenario.
Hopefully, it would never happen, but
you never really know because all this
stuff is so new. So, it's like I'm never
going to be like, "Don't do X, Y, or Z."
for you. I can do that because I know
you but like you know other listeners
are going to be like no I want to give
my health data to chd it helped me so
much like the you know medical system is
failing me that's fine. Yeah the medical
system sucks. So if you do want to find
patterns in your health records and you
feel comfortable with that level of risk
tolerance okay just if you do that of
course make sure you're doing it like
within like chapd health and not like
just regular chatbot but it's still
pretty risky. Yeah, I think I I to to
continue using the teenage analogy, I
feel like it's a little bit like the
advice you hear a lot of parents give to
like their their teenage children who
are sending let's say sensitive
pictures. Uh this this idea of like you
should assume that anything you send to
someone or create digitally will
eventually be public and that that is
your framework is is you shouldn't share
anything that you wouldn't share with
everybody. And I think every everybody
draws that line really differently,
right? And there's kind of no wrong or
right place to draw that line. But that
is it's a pretty extreme way to draw
that line. But given all of the stuff
you're saying, we just don't know now
and isn't regulated and isn't even sort
of industry accepted yet. I think that
there are a lot of ways that you know we
give a lot of information to Google and
we give a lot of information to Facebook
and whatever but there are at least now
sort of accepted norms in how that data
is treated and there would be real
problematic ramifications if that
changed. It doesn't seem like any of
that exists in AI right now. And it's
also hard because even if they do have
those protections in place um you know
like most of these companies do retain
some semblance of your data even if it's
like you know anonymized or the personal
stuff is stripped out they like use it
in some degree usually. And so, and
we've seen like in the past like 10
years, it's pretty easy to dean
anonymize data. And it's also imperfect
science on like what uh a system knows
is sensitive versus not. Like um you
know, Margaret Cunningham from Dark
Trace was telling me like it's hard for
chatbots to tell the difference between
a phone number and a social security
number or um you know, like a street
address and an account number. So, it's
tough because one, even if they're
trying their best, the like guard rails
here are not perfect. And even if they
do work, great. You can deanonymize
data. Um, you know, it's it's I don't
know for sure if you know, like a if
Chad beauty health like would be able to
do that, but I'm just saying like in
general, it's a pretty understood rule
that anonymization systems for, you
know, protecting personal data are very
very imperfect. So, it's like, you know,
you just really need to know the risks
here before you make a decision. And if
you do want to give your data over,
that's fine. It's just you need to do it
in an informed way without just like,
you know, taking off your seatelt and
being like whatever. Like, you know,
this may come back to buy me in 10
years, but I don't care. It if you want
to be like that, sure, but do it with an
informed take. That's all I'm asking.
>> Yeah. I am very much of the the
generation that shared every photo that
anyone took at every party in college on
Facebook. Uh, and then boy did we all
learn several years later to go back and
pretty ruthlessly comb through all of
the pictures that we had shared on
Facebook. Uh, and it feels like this is
the generation that's going to go
through that exact same thing.
>> I remember I used to um climb on my high
school's rooftop with my friends. Like
it was like so fun. We would like go up
the trellis and just hang out up there
and I can never share the photos on
Facebook because uh my mom was a teacher
at the school. The the the building has
now been torn down so I can share that
story. But it's like, yeah, I mean, that
was like, thank God I decided not to
share those. But yeah, we were we were
just being super willy-nilly about
everything because we grew up in in the
age of the internet and we wanted to
have people writing on our walls and
like, you know, liking our Facebook
albums. So, yeah, it's tough. Like, I
think people should, you know, kind of
apply the same thought process here. Um,
even though it feels private because it
seems like a one-on-one conversation,
you know, we've seen like CHVD records
go public because like the link became
searchable, you know, and like company
execs were like giving financial data
from their company into the system and
then like any public person could search
it. That has since been fixed, but
things like that can happen and you
never know how, when, or why they're
going to happen because this technology
is relatively new. Yeah, one thing I see
a lot of people wondering about and and
being fearful of is this idea that these
companies are going to use my data to
train their models. Uh, and that's both
sort of the the facts of our
interaction, but also like like you
said, the the the important financial
data that I upload into ChatGpt is going
to be used to train the next version of
ChatGpt and that that is a a privacy
risk or breach in some way. What do you
make of that? How should people think
about what of their data is being used
to train AI models and what that means
privacy-wise?
>> It's hard because these companies will
be very careful with their wording, you
know. Uh so you never really know the
full extent um on how they're how or why
or if they're using your data to train
their models. like they will say for
example Chad GBD health they say
explicitly your health data will be kept
confidential and it won't be used to
train their AI models but does that mean
some um anonymized stripped down version
of what you say won't be in some way
>> used to train the models
>> don't know you know they they don't
that's the thing they don't really go
into the the how here um and they don't
have to because it's all voluntary. So
the other part of it is it can change
you know they can change their mind at
any time. So I would say you know if
they explicitly say for any given
service we don't use your data to train
our models you can be pretty sure that
they don't for the most part at least um
for that but that may change and
certainly not for the ones that they
don't explicitly say that. The other
thing to keep in mind here is that if
it's a free product, you are the
product. So like, you know, if you're
paying for a product, there's less of a
chance they're using your data to train
or at least to a lesser extent. But if
it's free, like all bets are kind of
off. That's what we learned with
OpenClaw and what we've learned with
like a million products way before that.
But yeah, I would say you're a little
bit safer if you're paying. And if
you're an enterprise user of something,
you're way safer. um you know, Chadvd
Health specifically, you're pretty safe
because they're pretty explicit about
that stuff. But that's just for now. Who
knows? Um you know, Anthropic has a
similar product uh like that's HIPPA
compliant, but still like you know,
that's you're not bound. So, I don't
know. It's it's just a tough thing. You
need to kind of operate with with a
little bit of a grain of salt here.
>> Yeah, that's good. Can I can I read you
one that I found that just like flumxed
me forever? Uh and I think is is a good
proof of what you're talking about. So
this is from Anthropic's uh terms of
service for Claude. It says, "We do not
train our models on your Gmail or
calendar integration data, ensuring your
private information remains private."
Simple, straightforward, right? Again,
this is so much of this comes from, I
use Claude. I like the idea of being
able to pull information from my Google
Drive and my Gmail in Claude as I look
for things. Do I do this? So I'm I'm
reading this looking this up. That that
sentence makes perfect sense, right? We
do not train our models. This is the
next sentence. Note, if you are using
our consumer products, eg Claude, free
pro and max, when using claude code with
those accounts, that was a double
parenthesis I just read to you by the
way, and you have chosen to allow us to
use your chats and coding sessions for
model training, then any content you
copy paste from your Gmail or calendar
or claude responses, which include
specific information from these
integrations may be used to improve our
models. What [laughter]
>> exactly? This is what I'M SAYING. IT'S
LIKE IT'S LIKE you can't really know.
They will be pulling all sorts of double
parentheses on you. they will be doing
double negatives. You just don't know.
So that that's my thing too. It's like
yeah, okay. Like maybe it's not going to
directly use your emails, but if you're
copy and pasting things from your email
into it, okay, it'll it seems like it'll
use that based on what you just said.
And um what if it's returning stuff from
your email and saying, "Hey, here's a
summary of all the emails you got
today." Okay, it seems like that's also
going to be used to train the boat. So
it's like, okay. Okay. And again, like I
said, they didn't mention enterprise
there. So, it's like enterprise is
pretty safe, but the consumer products,
you don't really know. It's just tough
because there's not a lot of hard and
fast rules here. And the fact that they
can change these things at any time. The
thing you just read, maybe that'll look
a little bit different in a week or two.
I have a current like a tracker set up
for when these companies change like
their mission statements. And like you
have no idea how often I see an alert
that's like, "Oh, this changed
slightly." You know? I mean, usually
it's something dumb like they took out a
couple parenthesis, but sometimes it's
not. So, yeah, I think it's like it's a
we should treat these documents as like
living documents that are drafts and
constantly changing. And if you're not
okay with uh you know, that policy
looking different in a couple months,
you know, air on the side of caution is
what how I operate. Yeah, that makes
sense. Yeah, I think one really
interesting outcome of this whole
experiment for me has been that it it
all sort of leads to Gemini in a really
funny way because uh Gemini offers a lot
of the same things, right? Gemini can go
find your YouTube information. It can go
find stuff in Google Drive. It can go
find stuff in Gmail. It can find stuff
in your calendar. Um and I found the
same thing in Google's terms of service.
It says when enabled, Gemini accesses
your data to answer your specific
requests and to do things for you. And
because this data already lives at
Google securely, you don't have to send
sensitive data elsewhere to start
personalizing your experience. This is a
key differentiator. Like I think that's
true. This this is such I I wrote this
thing a few weeks ago about how Gemini
is is winning. And this to me is one of
the key pieces of it that's like, okay,
I feel uncomfortable giving my email to
someone who doesn't already have email.
You know who already has access to all
of my Gmail is Google. And so this idea
that actually privacy ends up being a
win for Gemini is so against what I
would have expected coming into this
especially for a company like Apple
which builds itself as the privacy
company but is going to ask for all
kinds of access to other data from other
platforms. Most of the stuff that I care
about already lives inside of Google. So
for better or for worse I have made this
privacy agreement with Google already.
And I think there are in an increasing
number of good reasons to get as much of
your stuff out of Google as possible,
but to the extent that you're
comfortable with the amount of
information that Google already has on
you, which for most of us is all of it.
>> Totally.
>> Gemini ends up becoming a much simpler
trade-off, right? It's like I'm giving
I'm giving my Google data over here to
Google over here, not crossing some new
corporate barrier.
>> Totally. And you know, just like with
this security of anything else, like the
more complex you make it and the more
organizations that have a hand in
something, the less secure it is. It's
the same reason why like, you know, if
you're really really trying to like meet
a whistleblower on the DL with like no
trace, you meet them in person
somewhere. Like it's like the just the
the more people that have a hand in
something, the less secure it is.
Actually, I was watching Tell Me Lies
this weekend. Same thing. The more
people that found out about a secret,
the it leaked the next day. So, yeah.
Yeah, I mean I think I was talking to a
guy um at check marks, Darren Meyer, and
he said like we have a history in tech
of giving our data to an organization to
get something of value like a trade-off
and then when we find out later that
they used our data in a way we weren't
okay with we get upset and AI companies
haven't done anything to show us they're
any different and in fact are probably
even more so like that because they
haven't been time tested. So, it's like,
you know, yeah, I think it it is
interesting because I've always thought
when you keep most of your stuff in one
ecosystem, um, you know, the AI agent
that can help you parse that ecosystem
is probably going to operate better
because it's been trained in that
ecosystem and it's going to be a little
bit more useful and, uh, have less of a
learning curve, less friction. So, I
mean, it makes sense that, you know,
Gemini would be working well and be a
little bit more secure potentially.
>> I mean, it's such a funny thing, right?
is it is you can give good security
advice on both opposite ends of that
spectrum, right? There there is a a good
and reasonable case to be made that a
the best thing you can do for your
personal data is put it a lot of places,
right? So that you you have fewer the
risk of each individual sort of vector
of attack is smaller and the possibility
of something going wrong in a huge
catastrophic way goes down. I mean, you
hear these stories about people whose uh
Gmail accounts get locked for whatever
reason and their life falls apart,
right? That like if you if you have all
of your stuff in one place, all of your
eggs in one basket, if something
happens, it's a disaster. So, put your
stuff in lots of places, it's better.
The flip side of that is [clears throat]
now all of a sudden if if you believe in
an AI future, what I'm actually doing is
then recentralizing all of that stuff
into a new place,
>> which is a new vector for problems,
>> right? I think that it kind of depends
on how many services you're connecting.
Like so, you know, if you have all your
stuff in different places, yeah, that's
clearly more secure. And each company
has less of a complete profile on you.
But if you're connecting Google to
Claude and you're connecting Google to
um ChachiBT, then it's like even more
companies have a fuller profile on you.
So
>> now trusting three companies with
everything instead of three companies
with a little or one company with a lot,
>> right? I think of it kind of like
diversifying like your assets or
whatever like financially like you know
if you like sure if you have everything
in like different banks great but if you
have like and this can't happen so this
is a bad metaphor but if you had
everything in every bank okay that's
kind of worse so yeah I think it's the
same thing here it's like you know for
example okay last week you and I were
laughing really hard on like the chatbt
trend on um asking it to create a uh
caricature of you based on everything it
knew about you and your top. Now, I
often use child GPT with the memory
turned off like logged out because um I
just don't really need it creating like
a intense profile on me. Do I have
accounts where I let it? Yeah, because I
need to test it and I'm an AI reporter.
But, you know, if I'm just like doing
something random, there's a lot of times
that I'm like, you know what, I don't
really need this to go into like my the
the understanding of me and and what I
want. And so, um the account that I used
to do that, I had only used it for like
five conversations like that were
recorded. And so it didn't have that
much info on me and it did generate me
in a hilarious way like just like travel
like Paris um string lights like it was
like very um basic but yeah I'm like it
only had five conversations to go on and
some of them were like wedding planning
tips so who knows um yours was of course
like more tied to like your work but um
it was just funny to see like you know
that's a kind of good example of you
know the type of profile that these
companies are building on you based on
your conversations. and what itself
knows about you based on everything you
put into it. So it's like you got to
think
about everything.
Yeah. So it sounds like again everybody
can make this decision for themselves.
Lay your privacy framework where you
want to. You know me, you know what what
I do and think about and the chaos that
is my computing life. It sounds like you
would tell me that I should probably not
put all of my Google data into Claude.
>> I think not, but I also understand the
temptation because emails suck. As you
know from other conversations we've had
on this podcast, I have like 13,000
unread emails in one account. So,
>> cuz you're a monster. [laughter]
>> Yeah. So, I get it. Um I think that like
you know if if it's not that much like
personal stuff like or not that much
sensitive information you could connect
it like you know if it's like work
related stuff that's like you know more
sensitive that's when I wouldn't ever um
for you but like you know if you just
have like your personal Gmail in there
and it's like a lot of appointment
reminders and stuff like again like for
other people maybe I'd say no but
knowing that you value like the ease of
organization and stuff so much. I would
say like go for it if there's not much
sensitive information in there. But also
it's like we all have so if do you
delete your emails for example like I
keep every email I've ever had and I
just like pay for the two terabytes of
data or whatever um even in my personal
account. So it's like the amount of
stuff goes back so far I don't even know
what's there. I wouldn't connect it. But
if you like delete emails when you're
not using them and stuff and like you
keep it pretty like manicured. Why not?
>> Yeah. Yeah. It's an interesting way
because I do think my use case is
exactly what you described, right? Like
I just want to be able to ask Claude
what my Delta frequent flyer number is
and it can tell me by finding it in my
email. Like that's fine. But what you're
also making me realize is I remember
when uh Alexa and Google Assistant were
first coming out, my running theory was
that actually what we need is not one
all-encompassing
AI assistant that everything funnels
into. That is like I use Alexa to talk
to my TV and to my speakers and to my
car and to everything. But that actually
eventually what we're going to have is
this like incredibly distributed thing
where every tool is going to have
something that is more specific to it
that like instead of addressing Alexa to
talk to my TV, I just address my TV. And
that is we got part of the way there
with some of the voice assistants but
not all the way there. And I wonder if
maybe I should be rooting for that
outcome with AI too. Is that rather than
connecting everything to Claude that
there should be I should use Gemini for
my Gmail and I should use something else
for my television and I should use
something like that maybe the the future
is many AIs and not just one.
>> I think that that is a lot more secure
for sure and sometimes more efficient. I
mean it's you can argue either way. In a
way, it's less efficient because
obviously not everything's in one place,
but also like you know chat bots on
certain systems are going to work better
if they've been trained in that
environment. Like we've learned that
through like tons of like RL research
and stuff. It's like the environment you
train in is the environment you operate
the best in. So, you know, like Gemini
probably would operate better within the
Google ecosystem and you know that makes
sense. Same with like a chatbot for your
TV that would like train specifically on
only those use cases. So, I mean it like
kind of like may lead to less friction
even though it's kind of annoying to
have everything in a ton of different
places. It's also more secure. Like
yeah, I think you can't really go wrong
with that.
>> It does feel it also feels more if not
more secure than at least more
understandable, right? Where it's like I
can I I can make a clearer decision on
what my TV should know about me than I
can this sort of all-encompassing needs
to know everything about me. And this
goes back to openclaw, right? where it's
like, okay, if I'm just giving this
thing complete unfettered access to my
computer, that's actually a really hard
decision to make thoughtfully, you can
either just say, you know, YOLO, it's
it's worth it. And to your point about
the the trade-off we've made, these
companies are statistically speaking
correct to assume that we will trade
privacy for features and convenience. We
always have. Is that the right decision?
Has it been the right decision every
time? Often no. We have made that
trade-off every time. Everybody who has
ever bet that users will make that
trade-off has been right.
So I wonder if A that will turn because
AI is asking so much more or B if these
companies can just keep barreling
through knowing that we will continue to
make that trade-off. But but my hope is
that at least it becomes a little more
readable. Right? The idea that like I at
least know the trade-off that I'm making
in order to use this product feels very
hard with a lot of these AI tools. And I
think distributing it a little bit more
would at least make it more parsible
that way.
>> I totally agree. And yeah, I think
that's the most important thing is
knowing what you're giving up so that
you can actually make an informed
decision on like am I getting enough
benefit from this service to make it
worth it for me? Like everyone knows
everything's a deal. Like you know it's
a trade-off, but do you understand the
trade-off or not? And you know, these
companies need to make it crystal clear
what the trade-off is and in what cases
and not try to like try any funny
business with like the double negatives
and the double parentheticals. Like just
be honest. What are you giving up? And
let people make that decision for them.
And you know, a ton of people will be
like, "Yeah, I'm fine with that." You
know, everyone knows everything about me
anyway. Don't care. A ton of people be
like, "Whoa, I don't want to do that at
all." Especially because this company is
like only a few years old and I don't
know what is going to happen 3 years
from now, four years from now. Yeah. I
mean, and also like, you know, people
that I I remember when like fridges
became smart for the first time and
people were really worried that like,
you know, if you bought a ton of beer
and like not enough fruit that uh health
insurance companies would somehow get
that data. It's like you just I don't
know. It's it's you just need to know
the trade-off you're making and then see
if it's worth it for you. For me, like a
smart fridge, [snorts]
>> it's not something I really need. I
don't need to be seeing ads on the the
front screen of my fridge either, you
know? But like, you know, for an a
chatbot that's like parsing all your my
12,000 emails, maybe it's worth it. Who
knows? But like, yeah, you just need to
be able to accurately understand what
you're giving up.
>> Yeah, I agree. All right. Well, I should
confess here at the end that I already
connected my Gmail to Claude, and I am
now regretting that decision, so I'm
going to go undo that for now. Uh, but
Hayden, thank you as always for being
here. This this is great. I appreciate
it.
>> Thanks so much.
>> All right, we got to take a break. We'll
be right back.
Support for the show comes from L'Oreal
Group, using the latest advancements in
science and tech to create personalized
beauty solutions for all. The global
beauty leader recently introduced two
breakthrough technologies that bring the
power of light to hair care and
skincare. Light straight and
multi-styler and the new LED face mask,
both of which were recognized as CES
2026 innovation award honores. Learn
more about both technologies on
laurel.com.
L'Oreal Group. Create the beauty that
moves the world.
>> Support for the Vergecast comes from
Wix.
Most AI website builders and vibe coding
platforms feel like a game of prompt and
prey. The results can be all over the
place and changing one thing can break
the whole design. [music] Wix Harmony is
doing things differently. You don't need
to pick between a free form editor or
vibe coding tool. You get both in one
place. It's the hybrid editor that's
bringing the next generation of website
building. Wix Harmony blends the power
of AI and precise drag and drop tools.
You can launch a professional-grade
website by entering a single prompt and
flow between prompting Arya, your
personal AI agent, and using manual
design tools that let you shape every
detail of your site. And you can rest
easy knowing that your Wix site is
backed by 99.99%
uptime and enterprisegrade security with
no add-ons required. Ready to create
your website? You can see why 280
million businesses around the world rely
on Wix for their websites. And go to
wix.com/harmony.
That's wix.com/harmony.
Support for the show comes from Samsara.
If your business relies on drivers, that
means you're used to relying on the
rules of the road. But road accidents
happen and it's tough to prove what
actually happened without footage. And
when that happens, your insurance rates
can end up spiking. That's where Samsara
comes in. It brings AI dash cams,
vehicle tracking, and asset visibility
together in one simple platform. Samsara
can help you protect your drivers, cut
costs, and operate smarter. Their AI
dash cams capture real-time video that
proves when your drivers aren't at
fault, protecting them from false
claims. It's trusted by over 20,000
customers worldwide, including major
companies across transportation,
construction, and logistics. Don't wait
for the next accident to take action. Go
to samsara.com/verge
to request a free demo and see how
Samsara brings visibility and safety to
your operations. That's
samsara.com/verge.
Samsara, operate smarter.
[music]
All right, we're back. Let's do a
question from the Vergecast hotline. As
always, the number is 866 Verge11. The
email is vergecasterge.com. We're not
that hard to find. Like, it's not there
are no good excuses for not hitting up
the Vergecast hotline. Do you know what
I mean? Here with me this time, the
Verge is senior phone reviewer, Allison
Johnson. Hello.
>> Hello.
>> I caught you just before you were about
to disappear into the beginnings of
phone season.
>> Yes. My family will not see me for a
week and a half. I'm going to be just
among the phones.
>> Yeah. So, it's it's Samsung Unpacked.
>> Mhm.
>> And then Mobile World Congress.
>> Yep.
>> Which is in Spain still?
>> It is in Spain.
>> Okay. And then we think potentially
maybe an iPhone like right after that,
right?
>> Yeah. Just for fun. Um, everybody was
just like, "What if an iPhone?"
>> You know, why not?
>> Yeah.
>> What if we iPhone?
>> Um,
>> let's do it.
>> So, our question is actually sort of
tangentially about this and and I think
is the kind of question a lot of people
either are asking or are about to start
asking very quickly about their phone
purchases. Let me just play this
question for you.
>> Hi, Vergecast. My name is Lucas and I
was wondering if I should do a kind of
midcycle upgrade for my phone right now.
So, I've got a 15 Pro Max. I've had it,
you know, couple years and it works
fine. It's a perfectly fine iPhone.
It'll probably be fine for another year
or two. But I'm worried that with the
price of RAM constantly going up that
the next phone I get is going to be
significantly more expensive. And I'm
wondering if I should do just like a
midcycle upgrade now to kind of future
proof so I don't have to worry about
something really expensive later.
>> So Allison,
agree or disagree that this question is
kind either is or should be on a lot of
people's minds right now? I totally
legitimate concern. Um I I'm gonna be
honest. I was in the kind of like oh
sure RAM is a problem but I don't build
PCs like so whatever kind of camp. Uh
our our friend and colleague Sean
Hollister wrote a great um article about
why the RAM crisis is coming for all of
us. Um so definitely check that out if
you haven't. Um, yeah, and I think it is
I'm already like fielding this question
on our internal Slack.
>> Um, you know, and people in in the
similar situation as Lucas that are sort
of like, I was thinking I would probably
upgrade maybe a year or two from now,
but should I pull that timeline forward?
Um, and I think the question is real. I
think that price increases in one way or
another are coming for smartphones um
and everything else apparently. So,
>> okay. So, I let's let's take this very
tactically in two directions. I think
the one is Lucas specifically
uh has a 15 Pro Max presumably wants
another iPhone and I think so 15 Pro Max
is now a two generations old phone. I
think you you would probably assume that
Lucas would be looking to upgrade,
especially if you're a person who's a
ProMax, not this cycle, but potentially
next cycle. Like I think I think Lucas
is probably going to buy like a 19 Pro
Max would be my guess.
>> Yeah.
>> Right. The skip the 17, that's fine. The
the 18 will be what it's going to be,
but like in in the normal course of
events where he's probably 2 years away
from an upgrade. And
>> but I think the question of should I buy
now to reset that cycle to give myself
four more years for this to get better.
>> What do you think?
>> I here's where I've kind of landed. I
think it is a factor to consider, but I
don't think it should be the only one.
The 15 Pro Max is an interesting case of
like yeah, you are probably after you're
more interested in in the latest and
nicest hardware, probably more so than
someone else. So that seems like a
factor towards um maybe upgrade a little
sooner. But I there's a lot of things
I'm unsure about how this is actually
going to shake out for prices. Um
>> Apple especially hates raising prices on
the iPhone. They'll do that sneaky thing
that they all do where they kind of like
just take away the lower priced option,
take the like lower um storage option
off the table, so they didn't really
raise the price, but you know, you can't
buy that cheaper version anyway. Um so
maybe that's not going to affect Lucas
directly, but there are all kinds of
pressures, I think.
um you know, the weirdness with tariffs
that's been happening. Um the RAM thing
and there's um it it's going to manifest
in ways that I think lead to a more
expensive iPhone, but I don't think I
think I think you should buy a new phone
when it's the time to buy a new phone,
you know, and um thinking of the the RAM
situation could be one factor in that
purchase.
Yeah, I think I agree. And I think for
for Lucas in particular, and the reason
I want to focus on on his use case super
specifically is I I did not expect my
advice to be wait, but I think the
answer is wait. Like the 15 Pro Max is
still a very good phone that will be a
very good phone for at least two or
three more years, right? like the idea
of it being sort of so vastly outdated
that the camera is not up to snuff and
that it can't do the things you want to
do. I think it's pretty unlikely in
let's say the next 2 years.
>> Um
>> and you know, knock on wood, it also
seems pretty unlikely that all of the
things that have led to this particular
RAM shortage being this bad right now
are also pretty unlikely to all be
accelerating at this pace still in the
next couple of years. Either the bubble
is gonna pop and a bunch of weird stuff
is going to happen or people will start
to ramp up the capacity to build more of
this stuff. Like one way or another, I
think we are headed to a not a permanent
RAM shortage. Um,
fast forward to 2029 and somebody plays
this clip back to me and reminds me of
what a I am. Like maybe
>> but it it does seem to me that like if
you're in the position of saying, "Okay,
I my phone is going to be very good for
two more years. Is that a risk worth
taking? I would kind of say the answer
is yes.
>> Um
>> where I feel differently is people who
are like, I was going to buy a phone
sometime in the next year or so, right?
People who are like, I'm not I'm not
tied to the upgrade cycle. I buy a new
phone when I need a phone.
>> Mhm.
>> Most of those people right now I would
tell to just go buy a phone.
>> Yeah.
>> Um do you agree with that?
>> Yeah. like um the the case in uh in our
internal Slack was um someone had a
Pixel 7a and
>> Perfect example.
>> Yeah. And I'm like, you know what,
you've gotten your money's worth on that
phone. I think you're in you're safely
in the zone of like upgrade now, upgrade
next year. And just adding that factor
of the RAM situation, maybe that tips
you toward like, yeah, upgrade now. Um,
but I yeah, I do think it's it it kind
of has to be time already, not sort of,
oh, maybe next year will be time or next
year I'm going to start thinking about
it. Um, that that's where I'm landing.
And like, you know, look, you can always
uh change out the battery. There's
always refurbished options, you know, in
a year or two if the flagship phone
prices are crazy. I think,
>> dude, I don't know. I do you remember
during the pandemic when the car prices
went nuts and not only did new car
prices go nuts, used car prices went
nuts? Like we it was we had a harder
time buying a used car than a new car in
like 2021. It was insane. And so part of
me is worried that like
>> everybody's going to have the oh just
buy a refurbished idea and actually
that's going to become a strange market
too.
>> Yeah. But in general, I think you're
right that it's not
if you're less of the of the mode of
like I need the best phone right now the
minute it comes out, you do have a much
larger set of probably more stable
options.
>> Yeah.
>> Um so is there anyone you would tell to
wait a a minute and like you're you're
we're about to go into phone season. Um
we're going to get Samsung phones. Um,
people are going to hear and watch this
on Tuesday. Uh, very soon after we're
going to get new Samsung phones. You're
going to MWC to see stuff. We're we're
hearing some inklings about, you know,
there's presumably more Pixels to come.
There's more iPhones to come. Is there
anything that you are like, wait,
there's something coming. Don't buy it
until you at least see what the new
thing is.
>> I think the good answer is like no.
Phones are kind of boring right now,
which like works in our favor, you know?
if this is a time when they're getting
more expensive, um it's just not as
important to upgrade every year, every
two years, even every 3 years, I think
you're you're fine, you know. Um so, and
I think that the manufacturers, you
know, we're already seeing this with the
I think the Pixel 10a was a very
iterative like hardware upgrade, maybe
in an effort to keep that price point
down considering everything. Um, so it
it's kind of a good thing that phones
are boring right now. And it might be
the case for a little bit.
>> That's a really interesting point
actually that maybe maybe the outcome is
not that your iPhone is about to get,
you know, several hundred more
expensive, but that actually the upgrade
from this one to the next one is going
to be even smaller so that they can keep
the price
>> in range. And I think that makes a lot
of sense.
>> Yeah. we might not see I think it's
pretty likely we won't see you know like
incremental increases in RAM every year
>> um the way we have been and yeah Apple
hates hates putting a higher price tag
on something so I think they will they
will pull all kinds of strings before
they have to do that yeah so okay I
think this this feels right so if you if
you have a device that you like and you
feel confident about for a couple more
years you can feel okay waiting but if
you're like oh I should probably Go get
a new Go get it.
>> Yeah.
>> Like, don't don't wait. Just go get the
thing.
>> All phones are good now. It's going to
be fine. Just go buy the thing.
>> This is how I feel. I just bought a I
bought new uh Sony headphones not that
long ago for exactly this reason. It's
like tariffs, all the shortages,
everything is complicated. Like, I need
a new pair of headphones. I'm just going
to go I'm going to go do it. I don't
need them this minute, but I'm going to
need them soon and I'm just going to go
do it.
>> Yeah. Yeah. We did that with the PS5,
which we were so late to the PS5, but we
were like, it's time to get one. and
then the price increases kind of came
up. It's like, okay, now's the time. And
I have zero regrets about that.
>> Yeah, that's a perfect example.
>> Yeah.
>> All right, Lucas, I hope this helps. Let
us know what you end up deciding. I feel
like given that Lucas has a 15 Pro Max,
the odds of him hearing this and going,
"Screw you. I'm buying a 17 Pro Max is
like pretty high." Uh, but Lucas, let us
know what you do. Uh, Allison, thank you
as always.
>> Yeah, no problem.
>> All right, that's it for the Vergecast.
Thank you to Allison and Hayden and
Boris for being here. And thank you as
always for watching and listening. As
always, if you have thoughts, feedback,
if you want to keep sending me stuff
that you're vibe coding, this has been
my favorite thing in my email inbox over
the last couple of weeks is I asked on
this show for people to send me examples
of things that you've been viating. And
I have heard incredible stuff. I think
at some point on the show, I'm just
going to sit here and just like read
people's emails out loud for 10 minutes
because you should hear some of the
stuff that other folks are building.
[music] It's it's so cool and so
interesting. And if you're building
something that you think is cool and
exciting, I want to hear about it. 866
Verge11 is the hotline.
Vergecasterge.com is the email. Keep it
all coming. This show is a production of
the Verge and the Vox Media Podcast
Network. And this episode was produced
by Eric Gomez, Brandon Kefir, and Travis
Laruk. I'll be back with Neili on Friday
to talk about all of the news. [music]
There's there's policy stuff still
happening. There's Epstein files stuff
still happening. Gadget season is back.
We got Samsung phones. We have a lot to
talk about. It's going to be awesome.
We'll see you then. Rock and roll.
[music]