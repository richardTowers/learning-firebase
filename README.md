Learning Firebase
=================

[Firebase](https://firebase.google.com/) is an app development platform. From
what I remember, it used to be "just" a realtime database. Then Google bought
it and — while I wasn't paying attention — they turned it into a full featured
platform for running applications.

Recently, I started mentoring a couple of self-taught software developers.
They both independently discovered Firebase, and they're using it to build
their first applications. That makes it worth learning about.

Getting started
---------------

To get started, I just have to sign in with Google. This is super low friction,
since I already have a Google account which I use with their other services.

Creating a project takes me through a "wizard" style interface where I'm asked
some questions about my project. For demo purposes, I think I'll make a little
app that compliments whoever logs in.

The wizard asks me about Google Analytics, which I don't want. Google are
making a clever move here. The more projects that use Google Analytics, the
better they're able to gather data on how people use the internet. At least
it's possible to opt-out.

Projects, apps, and plans
-------------------------

Once I complete the wizard, I can see my project. I get a default "Spark" plan,
which is free. There are [some quotas](https://firebase.google.com/pricing) but
it looks like you get a lot of cool stuff for free. I can see boring things
like storage on the pricing page, but also fancy pants things like machine
learning.

It wants me to "Get started by adding Firebase to your app". I'm going to stick
to a web app to keep things simple, but it's cool that native apps are given
equal footing in the interface.

It offers me "Firebase Hosting" for my app. That sounds like something I want.
I check that, and click register.

Firebase hosting - where's the client, where's the server?
----------------------------------------------------------

Now I see something I didn't want to see. It wants me to add some client side
scripts:

```
<!-- The core Firebase JS SDK is always required and must be listed first -->
<script src="/__/firebase/7.24.0/firebase-app.js"></script>

<!-- TODO: Add SDKs for Firebase products that you want to use
     https://firebase.google.com/docs/web/setup#available-libraries -->

<!-- Initialize Firebase -->
<script src="/__/firebase/init.js"></script>
```

I'm writing a web app, but I don't really want to be doing everything client
side... Can I use this thing to build a "normal", server-side app?

I reckon maybe I can ignore this for now. I'm not copy-pasting your script tags
Google. I do what I want.

It worries me that beginners get pushed straight towards building apps client
side - next thing you know they'll be knee deep in React, Redux, and who knows
what other complexities.

Adventures on the command line
------------------------------

Anyway. Ignoring that, it wants me to install the firbase command line
interface. Seems reasonable.

npm tells me that it:

```
found 4 high severity vulnerabilities
```

Nice.

Another day in the life of a nodejs developer. Looks like there's [an issue in
a deep dependency](https://github.com/googleapis/google-p12-pem/issues/297).  I
strongly doubt it's a problem for a tool like this, so I'll just ignore it and
press ahead. Again, I worry about this experience for beginners - are they
supposed to just ignore security warnings? Is that something we should be
training them to do right from the start?

Once that's installed, I need to log into Google again. This time I'm going
through an authorization flow to allow the firebase command line tool to access
firebase on my behalf. It's a pretty nice experience. Everythings so much
easier when there's just a single way of logging in to things.

`firebase init` brings up another wizard, this time in the command line.

First question, which features do I want? I'm not sure. I want a database and a
server I guess. But I seem to remember seeing that the "Realtime Database"
isn't recommended anymore, and that I should use Firestore. I think I'll stump
for: Firestore, Functions, Hosting, and Emulators (because they sound cool).
Let's see how that goes.

```
Error: It looks like you haven't used Cloud Firestore in this project before.
```

Sigh. Okay, off I go to set this up in the web interface... Only, the link
doesn't work? Ahh, I know what this is - the link follows a rederict chain
which ends up with a path that starts with `/u/0`. But I'm signed in to more
than one Google account, and the one I want to use isn't number `0`. So no link
for me. I'll find it in the web interface and add it by hand I guess.  The
slick user experience is starting to break down a bit here Google, I gotta say.

I wonder if it's going to ask me the same question for the other services I
want, which I also haven't set up? We'll see.

Off to the interface to create me some Cloud Filestore then. It gives me an
interesting choice between "production" and "test" mode. In test mode:

> Anyone with your database reference will be able to view, edit, and delete
> all data in your database for 30 days.

Wow. It's a pretty bold tradeoff to even allow that as an option. I imagine it
saves a bunch of faff working out how to authenticate, which is probably great
for beginners. I can manage a bit of faff though, so I'll go for production mode.

With that created, the CLI can do the rest. I get some more choices (TypeScript
or JavaScript? Do I want linting?). It offers to set up builds / deployment
with GitHub for me - that's pretty cool. It's another authorization flow to go
through though. I'm quite impressed that it doesn't hardcode the branch name to
`master` - I use `main` for new projects now, so that would have been an
annoyance.

Because I selected "emulators" I get some options about 

