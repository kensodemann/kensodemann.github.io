---
layout: post
title:  "Using Travis-CI to Run Your Ionic Unit Tests"
date:   2017-05-22
categories: angular TDD testing ionic
---

Now that I had [unit tests][unit-tests] running against my [Ionic][ionic] 
project, the next step was to get it set up in [Travis-CI][travis-ci] so
it will run the tests with each pull request and with each merge into 
master.

I figured based on the configuration I had to do to get the unit tests
running that this would be very similar in configuration to an Angular
CLI based project that I have building on [Travis-CI][travis-ci]. As it
happens, I was correct. All I had to do was:

- turn on the building for that repository
- copy the ```.travis.yml``` from my Angular project
- change the global package install
- change the scripts that are called at the end to run the tests
- profit!

Here is my complete ```.travis.yml```

```
sudo: required
dist: trusty
language: node_js
node_js:
  - "7"

apt:
  sources:
    - google-chrome
  packages:
    - google-chrome-stable
    - google-chrome-beta

before_install:
  - export CHROME_BIN=chromium-browser
  - export DISPLAY=:99.0
  - sh -e /etc/init.d/xvfb start

before_script:
  - npm install -g cordova ionic

script:
  - npm run test-ci
  - npm run lint
```

Good Luck!! And Good Testing!!


[ionic]: https://ionicframework.com/
[travis-ci]: https://travis-ci.org
[unit-tests]: http://kensodemann.github.io/angular/tdd/testing/ionic/2017/05/21/ionic-unit-tests.html
