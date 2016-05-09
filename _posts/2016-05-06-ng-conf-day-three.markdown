---
layout: post
title:  "ng-conf 2016 - Day Three"
date:   2016-05-06 06:01:00 -0600
categories: ngconf2016
---

## TypeScript & Angular

Big (or small, or medium ones that grow) JavaScript apps are hard. JavaScript is quick and extrememly expressive,
but is not built with large scale applications in mind. Enter TypeScript, which takes JavaScript and expands it,
transpiling it down to ES5 (or down to ES3 if needed).

In late 2014, AtScript was announced as the laguage for Angular 2. Microsoft approached Google to see what it would
take to make TypeScript the laguage that was used: Buffed up ES6 support, @decorators, SystemJS support.

MS was originally targetting just VS for tooling, but by working with Angular community that quickly expanded to
Sublime and then many other environments (Angular developers do not use just Windows and VS, but often use Macs or
Linux, etc.)

Since TypeScript is a superset of ES5, the tsc can be applied to ES5 code to find issues. An example was shown where
the existing ES5 was moved to a different folder and transpiled the JS to JS. Next the files where converted to TS and
the red squiglies were taken care of, which makes converting to TypeScript a very easy thing to do.

## Pipes

[Pipes][pipes] are a way to transform any type of data on the fly. Pipes are used like filters in HTML, and in many ways
are just like what used to be called filters, but are more than what filters were.

Some filters that Angular 1.x had have gone away, such as sortBy and the filter filter.

Given that change detection has changed in Angular 2, there may be some unexpected behavior with pipes that we
will need to deal with.

The presenter's code will be up on [her GitHub][yonet].

## Multi-Slot Transclusion

Standard Transclution - transplanting DOM nodes from another component, maintaining the scope of the original component.

Multi-Slot Transclution - like standard transclude, but can transclude more than one item. This is something that can
be done in Angular 1.5, but Angular 2 makes it super simple.

[The presentation][trans] is really short, you can watch it get the idea.

## Angular CLI

The [command line interface][cli] is built around the idea of getting started fast and enforcing the best practices standards.
No more setting up build and test environments. It is all done for us. The CLI also allows us to add
components, services, pipes, routes, etc. all while maintaining the build system and enforcing best 
practices with application structure.

The CLI has shorthand for a lot of the commands and options. Not important, just something to remember when using it.

The CLI is at a Beta stage as of yesterday.

## Miscellaneous Stuff

There were a lot of short presentations on day 3, and a lot of it just really has to be watched to be appreciated. Here are
some of the many things I enjoyed about day #3:

* If you don't like your job, the problem may [actually be you][hatework].
* There was a really good presenation and reactive programming and another on [reacitve angular][reativeng]
* There are many ways to [contribute to the Angular proeject][ngcontrib]. Just do it.
* [Deborah K][formdemo] had an interesting presentation on form validation that may be helpful to review, but I can't find it
* A short presenttion was given on Codelyzer, which we may want to switch to from our jsLint, jsCS tools as we move projects
to Angular 2. Codelyzer enforces common style. There are plugins for most major editors used for Angular development.
Codelyzer can also be used inside of the Angular CLI.
* The [closing Q&A session][ngqa] was really good, as always

[pipes]: https://angular.io/docs/ts/latest/guide/pipes.html
[yonet]: https://github.com/yonet
[annimationdemos]: https://github.com/matsko/ng-conf-demos
[annimationslides]: http://slides.yearofmoo.com/ng-conf-2016-slides/index.html
[cli]: https://cli.angular.io
[ngcontrib]: http://www.ng-contrib.org
[formdemo]: https://github.com/DeborahK/ngconf2016
[hatework]:https://www.youtube.com/watch?list=PLOETEcp3DkCq788xapkP_OU-78jhTf68j&v=cGnU3GaALOQ
[ractiveprog]:https://www.youtube.com/watch?v=yEeDbHvg1vQ&index=27&list=PLOETEcp3DkCq788xapkP_OU-78jhTf68j
[reativeng]: https://www.youtube.com/watch?v=mhA7zZ23Odw&list=PLOETEcp3DkCq788xapkP_OU-78jhTf68j&index=31
[ngqa]: https://www.youtube.com/watch?v=byoL6epzLKM&list=PLOETEcp3DkCq788xapkP_OU-78jhTf68j&index=33
[trans]: https://www.youtube.com/watch?v=59IY2MIl5u0&list=PLOETEcp3DkCq788xapkP_OU-78jhTf68j&index=21