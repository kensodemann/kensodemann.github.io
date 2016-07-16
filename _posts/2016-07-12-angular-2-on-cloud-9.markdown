---
layout: post
title:  "Angular 2 Development on Cloud 9"
date:   2016-07-12
categories: angular2
---

If you would like to learn to develop applications using [Angular 2][angular2] but either cannot
or do not want to set up the tools on your own machine you can always use [Cloud 9][c9] to
create a workspace for yourself and do all of your work in the cloud. Most of the
information I have here was obtained from [this video][moreinfo].

The video shows starting with a new workspace. You can do that, or you can create a workspace
by cloning an existing workspace. I almost always do the latter since I usually start projects
on my Mac and use [Cloud 9][c9] mostly from my ChromeBook. I pick up the basic steps to use
after you have created your workspace.

First, upgrade the node version. New workspaces start with version 4.x of node, which
in my experience works just fine, but you can take it all the way up to 6.x


```
cd 
nvm install 6
nvm use 6
nvm alias default 6
```

Next install the latest version of the [cli][cli]:

```
npm install -g angular-cli@latest
cd ~/workspace
```

If you are starting with a clean workspace, it is now time to create your application and
tell the [CLI][cli] to use a port that [Cloud 9][c9] likes.

```
ng new my-cool-application
cd my-cool-application
echo '{ "port": 8080, "live-reload-port": 8082}' >.ember-cli
```

This will create a subdirectory under your workspace, and you can do your work there.
I guess one advantage here is that you can create a single [Cloud 9][c9] workspace in which
you can have multiple applications, but I don't like to work that way.

If you started by cloning an existing repository, then your workspace is your application.
That is the way I prefer to work. One workspace per application, no needless subdirectories.
You still need to tell the [CLI][cli] to use a port that [Cloud 9][c9] likes.

```
echo '{ "port": 8080, "live-reload-port": 8082}' >.ember-cli
```

At this point, you can ```ng serve``` your application and use the [Cloud 9][c9] preview
functionallity to see it.

In order to run your unit tests, you will need to run the using the PhantomJS headless browser.
To do this, you need to install the browser and launcher, load the launcher plugin in your karma config,
and then tell the [CLI][cli] to use the PhantomJS browser.

Installing:

```
npm i --save-dev phantomjs
npm i --save-dev karma-phantomjs-launcher
```

Modification to ```config/karma.conf.js```:

```
    plugins: [
      require('karma-jasmine'),
      require('karma-chrome-launcher'),
      require('karma-phantomjs-launcher')
    ],
```

Finally, start the tests: ```ng test --browsers=PhantomJS```

[angular2]: https://angular.io
[cli]: https://cli.angular.io
[c9]: https://c9.io
[moreinfo]: http://blog.oasisdigital.com/2016/angular-2-cli-2-minutes-cloud-9/