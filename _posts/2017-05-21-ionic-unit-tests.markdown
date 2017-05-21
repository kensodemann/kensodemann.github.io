---
layout: post
title:  "Ionic Unit Tests (and end to end as well...)"
date:   2017-05-21
categories: angular TDD testing ionic
---

I love and respect the [Ionic Framework][ionic].  I have been involved in the development of several Ionic applications at
work, and have developed a Electron application using Ionic at home. This was all done using v1.x of the framework. One thing
I have never liked about their tools, however, is that unit testing seems to just simply not be a priority for them. I find 
this unfortunate. With the v1.x projects, I have worked around this by rolling my own Gulp tasks, but I never really got
around to playing with v2.x+ of Ionic until today, when I started playing around with Ionic v3.x.

Unfortunately, unit testing is still not something that is built in to their tools or templates. However, it looks like
it is something they are seriously looking at. Better yet, they provide [some helpful guidance][ionic-unit-testing] on
getting this set up. Here are my adventures following that guidance.

## Create the Application

I want to create a sample application that has a few tabs of basic information, so I started with the "tabs" starter template.
I am naming my app after a god of motion, and will use yarn for package management.

```
$ ionic start savitr tabs --yarn
...
$ cd savitr
$ git init
etc...
```

## Clone the Guidance

Ionic has a [git repo][ionic-unit-testing-repo] containing a sample application that has unit and end to end testing. I decided
to clone that for two reasons: I wanted a working reference that I could try, and I figured some of the stuff could just be copied
verbatim from there.

```
$ git clone git@github.com:driftyco/ionic-unit-testing-example.git
```

## Update my Application

The first thing I do any time that I embark on any change, especially a change like this, is create a branch for the the change.
This may seem like a waste of time to some people since I am working on this all by myself. However, if my efforts crash and burn
having them in their own branch makes it extrememly easy to clean up without effecting any other work I may be doing. Always use 
branches for your changes. It will make your life so much easier.

```
$ git co -b feature/unit-testing
```

### Install the Packages

I started my adventure by comparing my ```package.json``` with the one in the [sample application][ionic-unit-testing-repo]. That revieled
the handful of development packages I needed to install. I added the following packges to the "devDependencies" and then ran ```yarn``` to
install them (you could also run ```npm i```)

```
    "@types/jasmine": "^2.5.47", 
    "@types/node": "^7.0.21", 
    "angular2-template-loader": "^0.6.2", 
    "html-loader": "^0.4.5", 
    "jasmine": "^2.5.3", 
    "jasmine-spec-reporter": "^4.1.0", 
    "karma": "^1.5.0", 
    "karma-chrome-launcher": "^2.0.0", 
    "karma-jasmine": "^1.1.0", 
    "karma-jasmine-html-reporter": "^0.2.2", 
    "karma-sourcemap-loader": "^0.3.7", 
    "karma-webpack": "^2.0.3", 
    "null-loader": "^0.1.1", 
    "protractor": "^5.1.1", 
    "ts-loader": "^2.0.3", 
    "ts-node": "^3.0.2", 
```

There we also several ```npm``` scripts that need to be set up:

```
    "test": "karma start ./test-config/karma.conf.js",
    "test-ci": "karma start ./test-config/karma.conf.js --single-run",
    "e2e": "webdriver-manager update --standalone false --gecko false; protractor ./test-config/protractor.conf.js"
```

### Copy the Testing Scripts

Two directories can be copied (almost) verbatim from the sample application that you cloned. These are the ```e2e```
directory and the ```test-config``` directory. The former contains the end to end tests and test configuration. The tests
will obviously need to be customized for you application, but the sample test is minimal and thus will only require
minimal modification. The latter contains the unit test configuration scripts. It is unlikely that any of these
will need to change (until you upgrade to a later version of Ionic, etc).

```
$ cp -r ../ionic-unit-testing-example/test-config/ .
$ cp -r ../ionic-unit-testing-example/e2e/ .

```

### Copy a Unit Test

Start by copying one of the unit tests from the sample application. I suggest ```app.component.spec.ts``` since that component
should be fairly similar regardless of which template you used when you set up your application.

```
$ cp ../ionic-unit-testing-example/src/app/app.component.spec.ts src/app/
```

### Run and Fix Unit Tests

You are now ready to try the tests. The following command will compile your code, run your test(s), report success and/or failure,
and wait for further changes.

```
$ npm test
```

The ```app.component.spec.ts``` file has two tests:

```
  it('should be created', () => {
    expect(component instanceof MyApp).toBe(true);
  });

  it('should have two pages', () => {
    expect(component.pages.length).toBe(2);
  });
```

The second test case fails because the ```AppComponent``` does not have a ```pages``` property. All I am trying to do is
make sure the unit tests are set up properly. Just the first test case is adaquate for that, so I just removed the second
one.

### Run and Fix End to End Tests

In order to run the end to end tests, you need to be serving your application.

```
$ ionic serve
```

Now run the end to end tests:

```
$npm run e2e
```

The ```e2e/app.e2e-spec.ts``` file contains one simple test. All it does is check the title.

```
    it('should have a title saying Page One', () => {
      page.getTitle().then(title => {
        expect(title).toEqual('Page One');
      });
    });
```

The title when the ```tabs``` sample template is started is "Home", so change the test as such:

```
    it('should have a title saying Home', () => {
      page.getTitle().then(title => {
        expect(title).toEqual('Home');
      });
    });
```

## Conclusion

I am a little disappointed that [Ionic][ionic] still does not have unit testing (and end to end testing) simply built in to all of
their starter templates. However, it is very easy to add, and it looks like they realize the importance of it as a best-practice and
are looking at adding it to future releases, so that is positive news.

Good Luck!! And Good Testing!!


[angular2]: https://angular.io
[ionic]: https://ionicframework.com/
[ionic-unit-testing]: http://blog.ionic.io/basic-unit-testing-in-ionic/
[ionic-unit-testing-repo]: https://github.com/driftyco/ionic-unit-testing-example
[yarn]: https://yarnpkg.com