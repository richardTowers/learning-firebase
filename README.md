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

Because I selected "emulators" I get some options about those too. It downloads
some java code for them - write once, run anywhere, right? I'm sure they'll be
fine.

I'll commit the files generated by firebase init to git, so you can see what it
got up to:

https://github.com/richardTowers/learning-firebase/commit/8f9d83cd0086cb6ef0454a7894ef418180e50f33

Show me what you got
--------------------

Okay, so the wizards have done their magic. What have I wrought?

I can see that the GitHub Actions it created for me have run. So they've
actually deployed my code already - all I had to do was git push. That is
pretty cool.

```
    title: 'Production deploy succeeded',
    summary: '[compliment-machine-246ba.web.app](https://compliment-machine-246ba.web.app/)'
```

Google bought the whole `.app` top level domain, so they're get to use the
`web.app` domain. I have to say, that's a pretty nice domain.

Looking back at the web interface it looks like it's possible to choose your
own subdomain, to a degree. Phishy things like `google.web.app` aren't allowed,
but I imagine there are some pretty legit-looking domains you could get.

For now, the thing firebase has deployed to our domain is just a static site
that links to the firebase hosting docs.

Hosting
-------

The init script [created an index.html](https://github.com/richardTowers/learning-firebase/blob/f3c3e3585647fdb38e5bc75c01c48c9f79f4f2ea/public/index.html)
file for me in the `public` directory. It looks like this is the thing that's
getting served on the domain that firebase created for me. So if I change the
file, commit it and push to main, GitHub Actions should deploy an update for
me... So let's try removing all the gross, complex JS and start with something simple:

https://github.com/richardTowers/learning-firebase/pull/1

As expected, it automatically builds and deploys the code in the PR to a
preview domain. I corrected a little typo, and the correction got built and
deployed to the same preview domain. That's pretty nice.

When I merge the PR into `main` it deploys to the main domain for me. Nice.

Hosting dynamic content
-----------------------

I'm getting the feeling that Firebase wants me to do things on the client side.
Load some JS into the users browser, fetch some data, and make some changes to
the page. That's fine, but what if the user chooses not to run JS? Is it
possible to host an application on Firebase that works for these users?

The docs have [a section on serving dynamic
content](https://firebase.google.com/docs/hosting/serverless-overview). So it
looks like it should be possible.

You've got two options - "Cloud Functions" or "Cloud Run" - these look like
they're Google's brand of Function as a Service (FaaS) and Container as a
Service (CaaS).

I'm hip, so I'll use the FaaS thing - Cloud Functions.

Cloud Functions
---------------

I'll follow [the documentation on using Cloud Functions](https://firebase.google.com/docs/hosting/functions),
and create a little bigben function.

Small aside - the code snippet they give has a bit of a bit of a clanger in it:

```
const hours = (new Date().getHours() % 12) + 1  // London is UTC + 1hr;
```

... London's only UTC+1 in the summer, so this "Big Ben" is going to be wrong in the winter. Oh well, doesn't matter.

Anyway, back to the docs. I tried out the ol' emulators, and sure enough, I can make a request to:

http://localhost:5001/compliment-machine-246ba/us-central1/bigben

and I get back BONG BONG (which is right - I'm in London, and it's past 2pm). Cool.

I [raise a pull request for that change](https://github.com/richardTowers/learning-firebase/pull/2). I can see that GitHub actions runs, but it runs this command:

```
npx firebase-tools hosting:channel:deploy pr2-bong-bong-bong --expires 7d --project compliment-machine-246ba --json
```

which I think only deploys the hosting bit of the app, not the functions.

That's a little bit annoying - the cool preview bits are just for static
content. If I want dynamic content, that's Firebase Functions, not Firebase
Hosting. So the preview-channel deployments bit isn't so easy to set up.

Nevermind, I'll just deploy the functions by hand for now.

```
$ firebase deploy --only functions
...
Error: HTTP Error: 400, Billing account for project 'REDACTED' is not found. Billing must be enabled for activation of service(s) 'cloudbuild.googleapis.com,containerregistry.googleapis.com' to proceed.
```

Ah, fun. So I need to set up billing first? That's a bit weird, because the
"Firebase billing plans" popup suggests you get "Usage quotas for ...
Functions" on the free plan. Oh well, up I go to the paid plan "Blaze".

Some card details later, I'm able to deploy my function, and sure enough, it
ends up on https://us-central1-compliment-machine-246ba.cloudfunctions.net/bigben

... but now I'm suspicious - did it just need my billing information because
it's defaulted to the node 12 runtime? And the functions are only free on node
8?

Yep, a closer look at the pricing pages yields a link to the [frequently asked questions](https://firebase.google.com/support/faq?authuser=2#functions-pricing):

> Because of updates to its underlying architecture planned for August 17,
> 2020, Cloud Functions for Firebase will rely on some additional paid Google
> services: Cloud Build, Container Registry, and Cloud Storage. These
> architecture updates will apply for functions deployed to the Node.js 10
> runtime. Usage of these services will be billed in addition to existing
> pricing.

> In the new architecture, Cloud Build supports the deployment of functions.
> You'll be billed only for the computing time required to build a function's
> runtime container.

So you can still get some functions for free at the moment, but only if you use
the deprecated node 8 runtime. Got it.

Well, I'm a cheapskate, so I'll use node 8 and switch back to the free plan.
Honestly I don't know how a beginner could be expected to work this out. Also
node 8 is going away imminently ("Starting Feb 15, 2021, we'll no longer
support new deploys or updates of Node.js 8 functions"), so I guess the "get
started with serverless for free" deal is coming to an end.

Rewrite rules, okay?
--------------------

So this is all lovely, I've got some dynamic content running on the internet.
But my main application is running on a baller `.web.app` domain, and my
function is running on a much less cool `.cloudfunctions.net` domain. Users
aren't going to stomach that. There must be a way to get serve my dynamic
content on my cool domain.

Sure enough, going back to the [serve dynamic
content](https://firebase.google.com/docs/hosting/functions) docs, step 3
explains about rewrite rules.

https://github.com/richardTowers/learning-firebase/pull/3
