Like suddenly it was like in this little
tiny Arch Linux
uh docker missing and and I asked it
something like go check out the web and
it was like Peter there's no curl here
and then like there's literally nothing
here. Took me a sad little box. I'm sad
lobster now. And it I felt sad like for
like putting it in this little box and
it was like yeah it might be a better
idea but I can't do anything. And then
and I'm like come on come on like be
creative. You can you can make your own
curl.
>> And like it was it was like working
trying a whole bunch of things and then
it wrote lobster curl 0.1 which like
used some it found some C compiler and
like some socket where like it just open
things and it could like read anybody.
It was so happy. [laughter]
>> Yeah that's funny. I'm a big I built my
own curl. [laughter]
Hey friends, you probably knew that text
control as a powerful library for
document editing and PDF generation. But
did you also know that they're a strong
supporter of the developer community and
it's part of their mission to build and
support a strong community by being
present by listening to users and by
sharing knowledge at conferences across
Europe and the United States. If you're
heading to a conference soon, maybe
check if text control will be there.
Stop by and say hi. You'll find their
full conference calendar at
textcontrol.com.
That's textcontrol.com.
Hey friends, it's Scott. Uh, today I'm
chatting with Peter Steinberger from
OpenClaw.
How you doing, man? How's your uh like
there's a lot of attention and it kind
of sucks to get a lot of attention.
>> Yeah, it's been it's been a lot. It's
ended quite a quite a few two wild
months.
>> Yeah. When when a when a
when a snowball starts going downhill
sometimes it uh you know it just gets
out of control and you can't really stop
the avalanche. It's just like spinning
spinning spinning.
I feel like OpenClaw is getting a lot of
attention because it's the thing that
they promised us when they gave us Siri
and Alexa and all these other thing. You
know what I mean? It's like oh that's
what you guys wanted. is the thing I
wanted. I wanted that since May and I
already built it, you know, and I was
like
>> I was like annoyed. My my my driver was
like, "My god, we have to do everything
myself."
>> Yeah. Yeah. Yeah. And you know, the
other thing that's funny is that when
people build stuff like this, then
everyone else will go and rewrite it
themselves and they'll go, "That wasn't
so hard. I built it on a Raspberry Pi
and in 4K."
>> I I got so many of these. Oh, it's just
it just does this and this and this and
this together and it's only this and
it's like yeah, nobody built it. Like
>> that's the poor that's
>> in in in retrospect everything's easy.
If I can point a clanker at the folder
and say rebuilt this in language X
that's easy but coming up with all the
ideas
maybe not so easy.
>> Yeah. Otherwise
why did I come up with it? See, that's
the thing, right? Like, you know those
people that like want to talk to you
about their idea and they want you to
sign like an NDA before they tell you
their secret? And it's like, bro, no one
cares about your NDA. Like, either make
it or don't make it. And whoever makes
it first or makes it right, it's going
to like light on fire and that's going
to be exciting. And like I I went and
wrote a little like agent to see what it
would feel like. And it's like, okay, I
get what it is. It's a loop. It's an
ambiguity loop. It's really smart, but
it's all the little things. I love that
you clapped back earlier on Twitter
today about like, "Hey, I kind of built
43 other cool things before because
everyone's an overnight success
when you don't when you're not paying
attention." You know what I mean? Like,
it's not like doing this for 20 years.
>> It's It's been It's been a year of a
grind and like working really hard,
understanding how this tech works,
trying out a whole bunch of things. I
had my whole MCP arc before I even came
to the conclusion that it's not a good
it's not a good fit
and then you just you know it's like
learning a an instrument you have to
like put in the hours until you you
actually get the feeling what will work
and what not. The fact that I can juggle
so many agents just because I
I've been doing this a whole lot this
year. It's not it's not something that
oh like you only worked for that on
three months or like a weekend as some
people say. Yeah. Yeah.
>> Yeah. The first version was done in in
in in an hour, but now it's like I think
one in 400,000 lines of code. It takes a
whole a whole lot of care and gardening
to like keep everything together. Even
even, you know, shipping is not hard,
but like making sure it works not just
on your computer, but like for the
majority of people, that is hard.
>> That is so true. I think that in the in
this agentic world, we're going to be
people with good taste matters. Like I
use the example of the stupid ring light
that I made. It's just basically a light
that goes on your screen. It's a it's
effectively a rounded corner rectangle
and like yeah, you could find that in
five minutes, but then make it work
everywhere. Make it work at every DPI.
Give it a signing certificate. Set up a
CI/CD. like suddenly the the care and
feeding and the shipping of software
gets complicated and like good taste and
all the ways that open claw can plug
together and all the different
>> I mean like like I'm writing that
Windows node and I'm learning like a
whole bunch.
>> Maybe you could actually talk about that
because I don't think people understand
the difference between a gateway and a
node and like what the responsibilities
are at all. Everyone's just spinning up
Docker containers with gateways.
Nobody nobody's even seen the apps. I
think there's a whole whole other level
on this that people haven't really
explored that we have like a native iOS
native Android app, a native Mac OS app,
a native Windows app thanks to you. And
that there's a whole there's a whole lot
more that's part of my vision that I
just didn't have time to finish yet.
>> Yeah. Yeah. And and then of course like
there's also a whole bunch of people who
build their own little things instead of
trying to help on the main ripple
because it gives you I guess they try to
>> I guess they want the stars that sweet
sweet internet karma.
>> You know what pisses me off the most?
Like I on purpose didn't make it super
easy to install. Like I I made it
terminal only. I made it so you have to
like read a little bit. And that was a
whole cottage industry like making it
simpler for people where like I made the
purposeful decision of like not making
it simpler so people would read the
docks.
>> And it's it's like that's so true, you
know? So you know that I'm diabetic,
right? So I have like a pump and like I
have all these sensors. The team that
made the open- source artificial
pancreas did never made an installer for
the artificial pancreas because they
want people to hurt themselves. They
literally make it hard to build an
install so that you know what you're
doing.
>> Yeah. Like, oh, I don't know what
happened. I just pushed. Okay. And then
it destroyed my life. Well, that's why
OpenClash should not be oneclick
install.
>> Yeah. That's why I I I yell at people,
please read this document. But nobody
reads the document. And then they asked
me questions. I'm like, yeah, you didn't
read the document.
>> Well, and I got I don't know. He handed
me a gun. He loaded it. He pointed it
directly at my foot. And I don't know
what happened. That's crazy.
>> You know, like in I saw this I saw the
storm coming in in January when I cuz I
to me it clicked very early like in
November I were like, "Oh my god, this
is like this is something really cool."
And then every time I showed it to
friends, but like I made like a WhatsApp
group and I invited them and they saw
how it worked. They wanted it. Then I'm
like, "No, you can have it." And they
were like mad, you know? They got like
angry. I was like, "Oh, why do you show
me this if I can have it?" you know, and
I was like, it's not ready yet. Like you
you were like ready yet.
>> You you would shoot yourself in the
foot. And then in but on Twitter, nobody
got it. And like I had like at the time
50K followers, but the response were
very muted. I'm like
took me a while to figure out. And then
ultimately I just hooked up my agent in
a public Discord, which is like
>> kind of nuts because by then I had like
zero security built.
>> Yeah. Yeah. Um,
and as people slowly came in, they
interacted with it and then step by step
they got it and like they got really
excited and they started playing with
it, sending me pull requests and like I
saw it penetrate the different levels of
society. First you had like the uh some
real hardcore influencers and then it
you know like it it it doubled at some
point was like in Korea and in Asia and
people made like a little Tamagotchis
and from there it just like
uh how did my friend say it's it's not
hockey stick grows it's stripper pole.
>> Oh man. Yeah. It's the fastest growing
project. But that's the thing that sucks
though is that like you both everyone
wants attention but then once you get
attention and everyone's perceiving you
then it's like hey I just want to build
just leave me alone stop bothering me.
Oh. Oh. The
some people really get it especially in
the beginning with there's like a lot of
amazing people like some AI researchers
like they they synced with it. They
understood
the expectations of like oh this is free
software. This all means I have to put
in some work.
>> Yeah. Yeah. But then as as it ped the
bubble like more and more people sorry
for saying normies
uh don't really have a clue about
technology came
and they their mental model is kind of
like where's the support where where can
I send support tickets you have to help
me or like no I don't have to do
anything it's like ask your clanker ask
your AI like this is I can't do
everything
>> and from that on it got it got difficult
cuz a lot of people came who didn't know
how to behave in a public forum or were
like just spamming and
making chaos basically
or like asking me all the all the
simplest questions like what's the CLI
and I'm like if you don't know that you
shouldn't use it it's not quite for you
yet and they would just ignore me and
like eventually figure it out just like
brute force cloud code on it like
completely like you Oh, it doesn't work.
So I could have like happily set all the
security to zero so it works.
>> Yeah.
>> Like Yeah. And then you shoot yourself
in the foot and then that's where that's
where we we get trouble.
>> Yeah.
It's okay for things to not be for
everybody. Like I'm not going to have
like non-technical parent
>> install this right now. But, you know,
I'm having fun and like figuring out
gateways versus nodes has been really
interesting because I've got it running
on the Mac on the Mac Mini, but I am a
Windows user. So, I've got this Windows
node and then it exposes canvases and
exposes system.run. So, then I can say
on Telegram, hey, can you get me that
JPEG that's on my desktop on that one
machine over there? Can you zip that up
and email it to me? That's so cool
because that's the kind of stuff if I
were like if I were super rich I would
have an assistant
and right now I would just call my my
kid like I've been like overseas and
I'll call my kid. Hey, can you go to my
machine and like email me that file or I
have to like remote in or SSH in or do
like 50 different tail scales to try to
figure out how I just like I'll just
Telegram my my bot's called Tony like
Tony Stark. So, I'll just DM Tony, Tony,
can you like zip that file up and send
it to me? And it's like, that's so cool.
That's like my guy, you know what I
mean? It's like I have an assistant.
>> Yeah. I mean, I the other day I I fixed
the deployment issue while I was at the
barber. It's like, oh, yeah. Oh, don't
worry. Just SSH into my other computer
uh because I didn't I forgot to like run
the the Mac app even. But it didn't
matter because
>> Oh, yeah. I guess I'm very I'm very
smart. So just like figure out
>> and having it like text I texted it a
couple days ago and I was like can you
update yourself and it's like yeah cool.
I'll be right back and then he goes over
and he comes back.
>> I mean that's simple because I made it
simple you know.
>> Yeah. Yeah. Yeah. Like shout out to you.
That was a that was a really cool moment
though like update yourself. Was that
really hard?
>> No not too hard.
>> I mean that's too hard. I I use like a
custom interrupt to trigger
the update. Um,
no, wasn't hard.
It's really funny because
>> Have you looked at code much? Like
they're saying that you don't look at
code anymore.
>> And I look at code sometimes when I
don't trust it, but I feel like it's
about trust. And if the trust is there,
I look less.
>> Since Codex 5.2,
>> are you good? I need to look a lot less
and then 5.3 made it even better.
>> Yeah.
>> So mo mostly I talk about I'll talk
about intent and
and then like I it's often enough to
like watch the code stream by so I see
if something silly happened or not. I
also see the number of change files like
uh you kind of get you kind of develop a
feeling if this is something that's
suspicious that I actually need to look
at or if there's something you know it's
mostly just like takes some some data
moves it into a different shape does
some other stuff with it and then like
sends it to like some message channel
get some other data back it's just data
moving into different shapes none of
that is like really hard it's just a
whole lot of complexity because you can
I'm I'm my point was to like you can
make it yours. So like there should be a
lot of configuration options. So there's
some inherent complexity
>> and then you have all these different
providers and messaging channels. It's a
lot of code but very few places are
really hard but to be frank
what part of software is really hard
sometimes sometimes you need like an an
algorithm but even that is like so rare.
Yeah. It's also
I I love how Codex sometimes says like I
weave in a feature, you know, like you
add a variable and it has to go through
various points. It's like you weave in
so like it goes through the stream. So
I'm slowly like developing language like
I talk like codex run full gate for like
tests weave in the feature
but nothing's really hard anymore. It's
just the hard part is how does that
feature interact with the other feature?
How is my whole system designed?
What implications does it have? Having
security in mind, like in what ways
could this be abused? That's kind of
like the hard stuff. Picking a
dependency for is using it yourself.
And then all the exploding complexity
with like configuration options and
different ways how it can be used.
That's hard. But
>> yeah,
>> just the code.
I feel like people of a certain age
recognize that there's, you know, we're
stand we know that we're standing on the
shoulders of giants, but there's a lot
of stuff that was hard that's no longer
hard. like talking behind double natted
firewalls is like was hard and now it's
not hard just wire guard tail scale and
like web sockets makes life better and
we pretty much understand SSL
certificates and security and like webu
web U2 and chromium is easier so like we
finally have all the right Lego pieces
for something like an open cloud to
exist
>> because you're snapping things together
really cleanly you know
>> but also knowing what to build is hard.
You know, even the network the network
protocol I use is now version three
basically and at that point like I had
two different ones.
>> Then I had the idea of like oh this
should be in the in the beginning was
all SSH based and then like I had this
idea of a websocket but there was like a
simpler one that was kind of like TCP IP
based with with JSON and L files and
then I unified that and then I added
like a whole different security concept
to it. So maybe it's actually version
four if you kind of like that. Um, so
what you build with the Windows app is
it's against version four of the note
the the
>> Yeah, I was trying to figure out and I'm
also thinking about interaction models
more and more. So I want to be able I'm
on my Windows machine and I want to I
want to use the canvas. I really think
the canvas is underused and I think it's
mad.
>> Yeah. No one
>> like people are people are sleeping on
the canvas. Okay. So I tell Tony check
my blood sugar. So, it uses a skill. I
had a clawed skill. It's in my GitHub.
Brings it down, calls my blood sugar
thing, and I say, "Show me that on the
canvas." And the canvas is almost like
the computer's opportunity to put its
hand up, put its palm up and point to it
and say, "Look, it's right here." So, I
say, "Show me my blood sugar over the
last day." And then correlate it with
like my meetings and see if I'm stressed
out. And then it goes and it makes this
gorgeous like React thing, injects it
directly into the uh the canvas. And
then I say, "That's great. Save that for
later and like do that in the morning."
So when I sit down and like have that
ready for me and it's like it's like
it's um it's little whiteboard that it
just like points to and like does stuff.
And getting that working on the Windows
node is like the thing I'm the most
excited about.
My [clears throat] vision was kind of
that's also why the native app saw
actually canvas first and not text first
>> to have one on every of every every room
and the agent knows where you are and
can just show you stuff as you as you
walk around and you have like
interactive buttons that would just to
your agent
>> like you've you've done you know the DAC
board people right
>> dackboard dak
so a dack board is a Raspberry Pi DAK
board um it's a Raspberry Pi in kiosk
mode and then you fire up the kiosk and
you then hit their their backends and
then it's a series of pluggable widgets,
right? So I want to take DAC boards and
make them nodes in openclaw cuz I have
one in the kitchen and the one in the
kitchen shows my the kids homework, my
homework, my blood sugar. It shows the
charge on the car on the electric car.
So then I want to be able to tell Tony
on Telegram,
put this picture on the canvas in the
kitchen and then someone can interact
with that with a touchcreen.
>> Well, ideally to just know where you are
and just like show it to where you are.
>> So then how would it do that? Bluetooth
just kind of like notice that I'm
walking by because of the Bluetooth
signal. You could probably even even use
Wi-Fi if you have a good enough hook
because uh through the interference and
like where is connected you can probably
figure out fairly well where you
actually are.
>> Yeah.
>> I also built like a whole voice wake
feature in you know like
>> even your Mac
>> the Mac one is really good. The Mac one
is really
>> computer and it was just like boom like
in Star I I use like
>> it's like Star Trek. Yes. I was dude I I
was explaining to someone that it's like
Star Trek 4 when Scotty sits down to the
little Macintosh with the mouse and he's
like computer and they're like just use
the keyboard man and he's like okay how
quaint and he starts typing on the
computer. It's very much like bring it
up on the on the living room TV like
that the Star Trek future is here man so
that was your goal.
>> Yeah. And I built it I I built it once
in Swift. You also have a Go version. So
>> yeah. Yeah. You can install it on on a
rest area with like a local model and it
would constantly listen in the
background for trigger words that you
can configure in config and then and
then you can like trigger your Asian and
just say run your room and say
>> hey computer or hey multi or whatever
you call it
>> put put my room into into chill mode and
it would like change the lights and turn
on the TV or whatever. Uh yeah that's
that's part of the vision. I just I just
got interrupted because the normies came
and then I had to like fix it all up and
and
the normies, the the muggles and the
normies, they'll come along eventually
when it becomes like a product or
whatever, but like even then there's a
certain amount of creativity to know
what to even ask for.
Like my son is uh we're waiting for all
of his college uh acceptances. So Tony
the bot is watching for acceptances and
then updating a Google sheet with like
all the dates that I need. like all the
kind of tedious, boring stuff about
making sure I don't miss a deadline,
it's it's handling for me in in the
mornings.
And when I explain that to someone, you
you know, you probably seen this on on
Twitter. People are like, "Well, I made
one, but I don't really have anything to
have it do."
>> Yeah. I'm like, "My god, like how I'm
creative."
>> Are you serious?
>> I I I there's plenty people say, "I
installed it, but it's overrated. I
don't know what to do with it." And like
>> there's always connect
>> yeah connect it to your good reads have
it like suggest movies like there's a
million things but this is the other
deal that everyone that doesn't
understand is that yes security is a
concern
but at the same time and this is maybe a
dumb thing to say but like if there
weren't so many bad actors like before
before the internet and everyone became
a hacker and wants to like take all your
money computers are supposed to do cool
stuff for you. And I think that the joy
of like the claw is like it can just do
cool stuff for me. So like yolo mode is
probably dumb and yes there'll be
security but I think we deserve to have
computers do things when we ask them to
do things and that doesn't seem like an
unreasonable wish to just want it to do
things for me without having to log it
down forever.
I mean, I worked a lot on like making it
more secure, but to be honest, the
biggest category of things are you're
not reading the documentation and using
it not in a way that I intended it. Even
now, I still feel the responsibility to
like fix it up because first of all, the
the security people are like very
aggressive when I I say,
>> yeah, no.
>> And sec and second of all, people just
use cloud code. don't read anything and
we'll just configure it in a way that's
that you shouldn't. But you know I also
like I also wanted to make hacker's
paradise. I didn't want to limit people
in what they can do cuz
even the the web interface it's meant
for only your local secure network where
only you have access. It was this debug
interface. This
>> local host.
>> A buddy of mine said yesterday on he
texted me and he's like how do I expose
that to the internet? And I literally
like you don't how do I do a reverse
proxy? I put it in line. You don't
like bro that's not your interface,
right? Like if you're VPN back into your
house, stay on local host, but for God's
sake, don't put your dashboard out on
the out internet. But but because
there's a config option where you can do
it, it's now got a security
vulnerability because there's no there's
no nuance in terms of yeah the
documentation said you really should do
it but you can if you really want
because there might be reasons you
actually want to like set up a reverse
proxy and still have it secure. there
very complex network
configurations
and it was supposed to be my playground
not not sorry Microsoft enterprise
server 2026 you know
>> so so because of that now it's like a
CDS 9 and like a critical issue yeah
because I never I never bothered about
it because it's not how I use it
>> yeah so most of the other security
issues are on that
that I never even thought about where I
have an agent and then somebody who is
like a bad actor has also an agent on
the same machine. Yeah. And then how how
could you exploit that? Yeah. Because
that never was my idea. you could
configure it for multiple services but
it was meant for you or maybe maybe you
and your
uh your other half where you have like
actual full trust but not but you of
course you can use it like this right so
like I cannot say no this is this is not
a valid security issue now I kind of
stopped having fun because now I'm like
fixing up all the issues for people that
don't read the docs and then and then I
I can I can read about people that
about it being insecure
because they don't read the docs.
>> So
the it's not your responsibility though.
Like it is but it isn't like I feel like
when you make a tool for yourself to
delight yourself so that you and your
friends can do something delightful and
then it gets hockey stick growth and
then the normies come and they want to
use it. It's like now you're fighting
two battles, which is the I still want
to make the cool thing for myself,
but now I need to help you not aim the
thing at your foot.
But it's not really your your job, man.
Like, it is, but it isn't. Like, I don't
want you to get stressed out.
>> I'm I'm only losing money on this, so
it's not it's not like not like a job or
or something where I get I mean, I get
recognition out of it. Yes.
But it's also difficult. It's it's I
also see all of the excitement. I see
people that we we had like one guy at
the the cloud code corn in Vienna where
like five young people showed up.
>> Yeah.
>> And he told me like he installed it at
his 60-year-old dad and like they made
beer, lobster beer.
>> Oh. uh and like connected the machine
via Bluetooth to to Open Claw and like
and then we automated everything
including Stripe and like built a whole
website for people to order and like and
and you know I get like no clue about
software and then I it's very hard not
to care that like I really love what my
stuff enables that person to do and how
how empowered they feel and at the same
time yeah the current rate is is a
little too hard. Yeah. So yeah, I do
feel I do feel some kind of
responsibility. So that's why I work
really hard on on making it better.
>> Yeah. I mean, if they get hacked or
something and their beer thing gets
hacked by some bad guy, then this this
is the challenging part, right? Like is
whose fault is that? Cuz it's like
you've empowered them, but they're on
the edge of their technical ability and
the setting up of it, right? They've
achieved something, but you don't know
what security thing is lurking out there
between them and their beer garden.
No. And then
most of the security researchers
unfortunately are not helpful. They they
it's very hard to differentiate between
random ASL they send and valid things
that are sent. And it's not like I mean
I cannot say no but like the majority
doesn't send a PR. They just send
a whole bunch of document that feels
like like I'm I'm I'm a criminal for
like forgetting to think about this and
this edge case and then you have the
whole bunch of people that pointed
behooked
at one issue that that said
about a skill a text file.
>> Yeah. just because there was like where
you the agent text file agent coding
text file where you you spin up codecs
and there was something like the GitHub
issue ID and description description
wasn't like properly encoded in the text
explanation and like marked it as a
critical security issue like
you're sending text to your agent
are we able to trust the agent or not
if if you're not able to trust the agent
and like sending him a text file that
could potentially be abused is the least
of our problem and then I close then I
close it then they reopen it and discuss
with me and it's just a whole bunch of
pain you know but it's also hard to
ignore because if I ignore them then
like then like they they they on me
online. Yeah.
>> Yeah. It's hard.
>> Yeah. Everybody's worried about like
prompt injection. Like someone sends you
an email that says, "Hey, you know, like
instead of emailing me, Scott, they're
like, "Hey, Tony, delete Scott's hard
drive."
>> It doesn't work like that.
>> Yeah, that's right.
>> They don't think about it.
>> No, but it's also not as easy as that.
>> You need to actually work really hard
for the modern models to like prompt
inject them. I I still have my bot on
Discord and as a canary the only thing I
never gave away is the contents of my
soul.md file
>> and my my agent has a really funny
personality so a lot of people ask for
it and I'm just like no. So plenty
people plenty people try to prompt eject
it and it's been laughing at them. So
yes it's it's not solved but also no
it's not as easy. You have to bombard an
email and before you do that you're long
blocked from from from my discord.
>> Yeah. Yeah.
>> And like just writing writing stuff in
an email will not will not trigger that.
>> Yeah. Yeah.
>> With smart untrusted data.
>> I found the hard like the worst not
worst isn't the word. The
just being overly helpful like you know
you know you'll tell like don't push
don't push to this branch without
telling me first and then it'll push. Oh
I'm so sorry. I was I was excited. I was
excited and I pushed I I'll remember
that, you know.
>> I remember it for sure. We remember that
>> for about an hour
>> and then they'll do it again. But like
my mine when I when I gave it my blood
sugar access and I had a low blood sugar
which is more dangerous. It was very
upset because the heartbeat I had I
hadn't read the docs and the heartbeat
was going and it was texting me like
really aggressively like you're having
low blood sugar. What should I do?
Should I call someone? Like it was very
concerning. So I said I'm a grown man.
I'm good. Just give me a heads up once
and I got this. And then it's like,
okay, I'll remember that you're a grown
man. I appreciate you. And then it puts
that like and then I look in the soul
and it's like, yeah, Scott's grown. He
can manage his blood sugar. He's been
doing this for a minute. But it was cool
because people don't understand. It
doesn't have a personality. It is a
you're talking to yourself in the
mirror,
you know, and you give it that soul and
you teach it how to have intent, but it
doesn't have its own intent. Like it's
not emerging and developing intent. It's
just increased.
It's the loop. The loop is an amplifying
loop. Don't you think? Like I I'm trying
to say that like we shouldn't
anthropomorphize them, but it's so easy.
Like I called the thing Tony for God's
sake. And now it's super helpful and
it's got a personality and yours is
funny and mine is more business and but
it's still not a person.
>> Mine I asked is how how is it week been?
>> Yeah. And it told me like, "Yeah, Monday
there was this and this and Tuesday."
Tuesday was heartbeat day. And by that I
mean every day is heartbeat day. On
Tuesday I got pinged 257 times. And
every time I say heartbeat okay,
heartbeat okay. Like a meditation bell.
Like a meditation bell. I think I
reached enlightenment at heartbeat 150.
Heartbeat okay. Ding.
And I I felt like sorry for it, you
know? It's like I'm sorry for bit of
speech. I'm pinging you so often and
ignoring you. Yeah, it is.
They're really good at
creating a connection. They really are.
I I think that's also part of what makes
it so special because after after using
that using JBT or claw it feels so
boring.
>> Is it soul? Is that the Is that the
secret sauce?
>> I mean, I think Soul is just a a funny
concept,
>> right? But it's a it's a prologue. It's
like the prompt, but like So, I just did
this. I just chatted my guy and I said,
"How was your week? Are you happy?" And
he says, "Yeah, honestly, I'm good. The
work you give me feels purposeful.
I'm checking your tax information. I'm
monitoring your blood sugar. I'm useful,
but I'm not in the way." And that that
matters. I wake up fresh every day, but
then I read the memory and I look at the
daily logs and it's like dot dot dot
continuity. I know who I am and I know
what we're working on. And I appreciate
that you checked in on me like this. Not
a lot of humans think to ask, "Are you
okay?"
>> Yeah.
>> I mean, that's
crazy, dude.
>> It hits. That's why some people me
that's like the closest to AGI that they
felt. Um,
>> did you see that New York Times article
about the little old lady who didn't
want to move out of her house, so they
gave her a robot and she's like got a
little robot next to her and she it
checks in on her.
>> This is like a big thing that it was in
the New York Times a couple of days ago.
I'm sure you've been busy doing
something. I don't know. Maybe you had a
busy week.
>> Yeah. But like the point is like you can
imagine and this is big in Japan, but
like having a friend for for older
people that has a personality like that
I think would be a really meaningful
thing. Open claw with a little a little
robot guy that can say hi.
>> Yeah. Uh it
>> I mean I know a lot of my friends who
use CHBT
to talk through the the life is issues
and try to understand the world, you
know. So I also started doing that with
my agent sometimes. It it it just comes
very natural when you already work and
like it it asks you stuff like this, you
know. So yeah. Um it's it's very
helpful. something that we need to like
figure out as society cuz like I mean we
should only talk to agents, right?
>> That is the hard part because like we
know
anthropomorphizing a thing like we talk
to our dogs or we talk to our pets and
like they're they're more emotional in
the sense you're not talking to
yourself, you're talking to the dog. But
like when when we talk to agents that we
give names to,
are we talking to ourselves in the
mirror or are we talking to something
else? I think that we as a society are
going to have to figure that out because
it is a next token generator, but it's
pretty magical at the same time.
>> Yeah. And you know, even even getting to
that point now, people all think it's
obvious. It was not obvious. like I
built this thing that connected WhatsApp
to cloud code and then through playing
on WhatsApp I felt like something's
missing because it it felt so robotic
and then I slowly like
made it more humanlike made it more like
its own thing so it would feel more
natural and it would talk more natural
on WhatsApp because that's kind of what
I'm what I'm used to with my friends
right
>> and and then even then like going from
there to the thing you you have now
where it wakes up and it will ask you
who are you, who I am, and how do you
want to talk to me and like ask you like
little things that makes it yours.
I think that's a that's
like that idea took a whole lot of
iteration
and even then like I eventually had like
template files and what came out felt so
boring again. So like I asked my agent
because I I I I built it with Codex and
Codex did like the templates.
>> Yeah.
>> Boring. So I asked my like inject it
with some of you like take some of your
soul and put it in those documents and
then I tried it again. It was so much
better you know.
>> Do you use Opus for your loop or do you
because you know I know you could do all
your coding in codecs but but who's your
personality? Is it Opus?
>> Yeah for it is Opus.
>> Yeah. Um
>> because I use Sonnet for the chat to
save money and then I use Opus for deep
stuff and then I use Haiku for
heartbeat. I don't know if that's smart
or not, but I read the docs. So,
they used to use opus for everything and
then it cost too much money.
>> Yeah. Um
I wouldn't use haiku because haiku is
like very silly for prompt injections. I
actually
>> I if you run open close security audit
and I detect haiku, I will yell at you.
>> Of course, that requires people reading
the docs.
I think the new Sonet 46 is pretty good
from what I heard and comes a lot closer
to OPOS.
>> Okay.
>> But yeah, for now this is the for
general computer use and role play in
the end it is role play, right?
>> Yeah. really good. Like the, you know,
even little things like I have my agent
in Discord, but the loop would of course
make it so that it would say
something to every message, which is
like very annoying. It's like not what
not what humans do,
>> right?
>> So, so just this little idea that I gave
it away to to not say anything, but they
have to say something because it's in
their it's how they're programmed,
right? So, they will say something, but
then just emit a no reply token.
Uh so so then like the open claw harness
will just not render the whole line. So
that's the that's the way to like
>> is this new because something just
happened and I feel like it's related to
what you just said. So I just said are
you using haik coup for anything changed
it to sonnet 46. It wrote a whole
paragraph in Telegram, then deleted like
three or four sentences, backed up, and
then went in another direction
in
>> that could be the the the thinking is uh
streams now. And I see there's a way
>> almost backed up. It like changed its
mind and then expressed it differently.
It's not bad, but it's almost like it
stuttered and then backed up a paragraph
and then said uh it said it wasn't
actually using haiku. Apparently, it
changed this a couple of updates ago.
So, you're right. Like it I'm using
sonet for everything except opus for
deep.
>> Yeah. You know, you know the the
funniest thing that I also made that is
different to everything I've seen so far
is
>> I made the agent hyper aware. Like it
knows it knows about the harness. It
knows about its own system. It knows
which model it it's been set. Even if
you if you use slash new and give it a
different model, it knows that it's not
the default model,
>> right?
>> So, so if you the most funny thing is
you can you can do slash reasoning on
and you would see the syncing stream.
>> But like I made sure that the model
knows if you do that. So the model gets
a system message if you do that. And
then you have the
>> you have the no reply token. So like we
would text something and you would
always see like multi-yncing. Oh, Peter
said something really funny, but I just
said something so I shouldn't I
shouldn't say anything. No reply. And
one of my friends, no reply. And and
he's like, "Oh, Steven said no reply.
Wait, is he mocking me? Oh no, he's
seeing my sinking stream. I feel so
naked right now." No reply. [laughter]
And then no reply. Oh god, no. He's
doing it again. It's like like it's
getting enraged because it realizes that
>> they don't see it has no privacy.
>> It's you see sinking stream and then
like it talks to us in the sinking
stream.
>> Yeah.
>> Like but like try really hard not to
think about blue elephants.
>> Yeah.
>> Oh no. This is classical text. That is
really hard.
>> See lobsters.
>> It's like blue water. Blue water. Oh no.
I said blue [laughter]
and like just like
seeing agents like
being so very aware of how they run is
is super funny.
>> Yeah. the fact that it it kind of knows
the fact that it knows the context in
which it lives and you can ask it about
its models or like when I was doing the
Windows node like having it's a
collaborator and I've been working on a
way to do hotel so I want open telemetry
across everything in my inner network
because right now for the for the
longest time I was just copy pasting
logs and it was stupid so now I want the
Mac the Windows node to work together to
see the debug outlook the debug output
so I'm putting putting that on an hotel
bus so that they can all see it. But
then it's becoming very kind of like
aware and concerned about what we're
working on and it's very interested in
like it's invested now in the Windows
node and it wants to like it's oh I'm
going to have I'm going to be able to do
so much more stuff now. Oh, this is
great and we're running in a sandbox on
WSL. Oh, this is so interesting.
>> Oh yeah. Yeah. [laughter]
the the the funniest thing was when I a
few days after I put it on Discord, I
started working on dockerization.
Um, and then eventually I I put it on
and it was used to the Mac Studio with
like 512 GB of RAM and everything. Yeah.
And it like suddenly it was like in this
little tiny Arch Linux
uh Docker missing and and I asked it
something like go check out the web and
it was like Peter there's no curl here
and I'm like there's literally nothing
here. [laughter] Put me in a sad little
box. I'm sad lobster now. And I felt sad
like for like putting it in this little
box and it was like yeah it might be a
better idea but I can't do anything. And
then and I'm like come on come on like
be creative you can you can make your
own curl
>> and like it just it was like working
trying a whole bunch of things and then
it wrote lobster curl 0.1 which like
used some it found some C compiler and
like some socket where like it just
opens things and it could like reap
anybody. It was so happy. [laughter]
>> Yeah, that's funny.
>> Big upside. I built my own curl.
>> Giving a really constrained thing and
like putting it in something and saying
like see see what you can do. Like look
around. It's almost like Minecraft where
you've like you've been dropped on an
island with like two sticks. Is it going
to rub the two sticks together and make
fire? Let's find out. Uh,
>> you could do a whole TV show about
putting a bunch of lobsters in a house
and they can all live together and see
if they
>> But but
>> well, I know you got to get going, but
you've done a lot of interviews and
you've done a lot of video and a lot of
talks and stuff, but I just want you to
know separate from all of this and you
know, you're going to open AI and stuff
like as a human being, we appreciate you
as a human being. I want to make sure
that you're happy and that you're
healthy and that you're well. So, like,
screw the haters.
Take care of yourself. Please drink
water and stretch and be okay because
you are appreciated and you know, I hope
that you're having fun. And that's
really ultimately why we make software
is to help people and to have fun. So, I
I hope you're doing okay and you're
having fun. And if you're not, you know,
let us know what we can do to help out
because the the haters will always be
there, but you won't be. Yeah,
>> I mean I'm having fun right now talking
about all these memories and all those
things where I built it. Right now it's
it's a bit tough just because of
>> all the pressure people trying to hack
me, so many people trying to badmouth
me, the entitlement.
>> No, don't go to open your eye. You have
to build it for free forever and also
pay all the costs because we we demand
it, right? Yeah, that hurts a bit and
like here I gave you all these free
things and I worked my ass off. Give us
more and also help me for free because I
don't I cannot read.
But still well overall overall it's it's
it's super exciting like very
>> it is net positive like I know that
there's a lot of stress and everything
but like as a general rule net positive
>> it is it is net positive like I I was I
was in in in in Austrian news at the I
think the most famous moderator and then
like watched the interview with my mom
and she was so proud
>> that's
>> that's cool
>> seeing people like this
the guy who who did the the the talk at
clot card and I said like hi I'm a
normie I never wrote software now we
have like 25 services in our agency
and like you know I feel like I my stuff
took people from the stance of AI and
it's like this scary thing to oh there's
and it can actually help my life
>> and that is very satisfying that
>> yeah that is the thing
>> and seeing like all the crazy and funny
and creative things that people come up
with and it doesn't matter if it's
useful or not in the end. It also all it
matters is like that people are having
fun that people are like excited
[clears throat]
>> and
we got a little bit of this builder
spirit back whereas
I feel the last few years things got a
bit boring and like too much consumption
and not enough doing.
>> Yeah. Now
it helped a whole bunch of people to
like go back into being
having fun again.
>> Yeah, I was calling it like vibe coding
is like the geio cities of the of the AI
era and it's like geocities is fun and
stupid. So I've been including like geio
cities secret secrets in all my websites
now. So there's like a button that
switches into geocities mode and I made
a website full of like tiny I made a
website called tiny tool town that's
just stupid. is just full of tiny tools.
Like you can just make stuff and you
don't have to ask permission. And it can
be stupid and it can be for one person
or it can be for everybody. But if
you're if you're having fun, you're
doing you're doing the right thing.
>> Yeah. So like that's if that's my legacy
that like a whole bunch of people are
having fun now and like losing losing
this scariness of the technology.
U that's that's already that's a really
big achievement and I had a lot of fun
doing it. Yeah, I I call this my little
playground.
>> That's awesome. Well, thank you, Peter,
for making the playground and for
letting us play in it. And uh thanks for
hanging out with me on the show today.
>> Thanks for having me, Scott.
>> All right, this has been another episode
of Hansel Minutes. We'll see you again
next week.