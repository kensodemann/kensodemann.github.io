---
layout: post
title:  "The Tools are Kinda Borked Right Now"
date:   2016-07-04
categories: angular2
---

I have a couple of small applications I am plinking with in order to continue my [Angular 2][angular2] learning.
I noticed that there were a couple of releases recently. Angular 2 is now up to Release Candidate 4, the [CLI][cli]
is now up to Beta 8, and [Material][material] is up to Alpha 6. So I started to update things, and found that things
are currently not playing together as nicely as I would like. I also discovered that there are ways around all of this.

## CLI Issues

The first issue I ran into came after I upgraded the [CLI][cli]. I got the following error attempting to create a
new project just to get out the new install: ```Error: Cannot find module 'exists-sync'```

It turns out that this fix for this is fairly easy (adjust the steps based on OS, the following is for Mac OSX, the
```sudo``` may or may not be required based on your permissions, how you install, etc):

```
cd /usr/local/lib/node_modules/angular-cli
sudo npm i
```

You also need to do the same for any local installations (that is, any project that is using the [CLI][cli]), only
there you go to the local install and you don't need to ```sudo```). Assuming you are currently in a project directory:

```
cd node_modules/angular-cli
npm i
```

If you want all of the hairy details on why this works and when it will be fixed, there is a [bug for that][iss1186].

## Router Issues

This isn't really a router issue so much as the fact that the router is _still_ changing, so you will need to re-do your
routing configuration yet again. Not a big deal, but you will have to do it. This time, the [Tour of Heros][TOH] tutorial
is up to date and helpful, so you can use that to help you out.

A bigger deal for me is that the [CLI][cli] Beta 8 sets up the application to use Release Candidate 3 and an Alpha version
of the router, which doesn't support everything that is in the [Tour of Heros][TOH] tutorial or the [API Documentation][apidocs].
I needed some of that functionallity. Specifically, I needed to be able to do this:

{% highlight JavaScript %}
export const ROUTES: RouterConfig = [
  { path: '', redirectTo: '/about', pathMatch: 'full' },
  { path: 'projects', component: ProjectsComponent },
  ...
];
{% endhighlight %}

But that ```pathMatch``` property was apparently not supported in the Alpha release. Sigh. OK. So, I manually updated to
RC 4 and the Beta 2 version of the router, but that led to yet another issue...

## Material Issues

One of my little applications is using [Material][material], and the material Icons component has a hard dependency
to RC 3. To get around this, I hacked its ```package.json``` file to make it depend on RC 3 or greater. That seems to
work.

## And Finally...

One of my simple little apps seems to be working just fine. I still have a couple of warning to deal with, but all of
the show-stopping errors have been fixed for now. I will have to try and update the contacts demo which I created for
a MKE user's group demonstration, but that one _should_ be easier to deal with since I do not have the
[Material][material] dependencies.

Oh, and don't try using ```ng generate route``` for now. It is intentionally disabled until the router development
calms down a bit, but it does give a link to some [interesting router information][router], so that is useful.

[angular2]: https://angular.io
[cli]: https://cli.angular.io
[material]: https://material.angular.io
[iss1186]: https://github.com/angular/angular-cli/issues/1186
[TOH]: https://angular.io/docs/ts/latest/tutorial/
[apidocs]: https://angular.io/docs/ts/latest/api/
[router]: http://victorsavkin.com/post/145672529346/angular-router