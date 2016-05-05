---
layout: post
title:  "ng-conf 2016 - Day Two"
date:   2016-05-05 06:01:00 -0600
categories: ngconf2016
---

Today is Cinco de Mayo, so tacos for breakfast!! Woot! Almost as good as yesterday being May the 4th be with you day.
It is also Fair Day, which means smaller more focused workshops rather than the larger presentations. I will try to blog
about the more interesting workshops.

## Keynote \#2

The second day keynote address had a couple of interesting items: a multi-year future plan for Angular Material, and 
Angular 2.x evergreen. Angular Material addresses the need for a standard UI library that plays nicesly with other
Angular aware UI libraries. The information on this starts around minute 50:30 of the [keynote address][keynote2]

Angular 2 evergreen addresses issues keeping everything upgraded, and dealing with the constant change and breaking changes
that will have to be part of any modern application. This starts around minute 58:00 of the [keynote address][keynote2].

## Migrating from Angular 1 to Angular 2

The [git repo][ngupgradeapp] for this session has the steps just in case I am too busy to write notes. Fork it then
clone your fork and follow the instructions in the readme. You can also view the [slides for this presentation][ngupgradeslides].

Examining the repo, a couple of things strike me:

* the starting app is heavily using Angular 1.5.x components, we are using Angular 1.4.x which does not have these,
one of the presenters mentioned that using components is not required, however, and this also works with directives
* the starting app is using a very light build process, whereas we have a fairly heavy gulp process, which may take some work

I believe it would be a good idea to walk through this project and exercises and think about how it would work for
our application.

## Ionic v2

The [git repo][ionicv2] for this session contains most of the information for this presentation.

Actually, there wasn't much of interest here. As soon as Ionic releases a version with RC versions of Angular 2 we should
look at doing a big-bang port of our eTime application, and perhaps even release that port after Angular 2 and Ionic 2
both go into production.

## TypeScript

[Everything for this talk][tses6in60] can be found in once place, includeing all of the slides.

ES6/ES2015 has several new features:

* Maps & Sets
* Modules & Classes
* Block Scope (tied to let and const)
* Destructuring
* Arrow Functions - besides the shorter syntax, these also preserve "this"
* Default Parameters
* Rest Parameters - nothing to do with REST

ES6/2015 is a superset of ES5. ES7/2016 is a superset of ES6/2015. TypeScript is a superset of them all. TypeScript is
compiled down to one of the ES versions (typically ES5) in order to run in the browser. TypeScript can actually be
compiled down to ES3 if needed, but all mordern browsers support ES5 so there really is no good reason to go lower.

TypeScript/ES6 supports importing "modules" which is similar to the module system used in Node.js.

{% highlight JavaScript %}
export var name = 'James';
export ver city = 'Chandler';

// in a different file
import {name, city} from 'customer';
console.log(name);
{% endhighlight %}

## New Data Architecture in Angular v2

[The Documentation][dataarch] for this workshop has all of the preliminary setup instructions and instructions.
This particular course was a bit of a bust. Just leaving this note here in case the link comes in handy.

## Libraries

The main part of this whole workshop was really just getting a project set up using [these instructions][library].

[ngupgradeapp]: https://github.com/thoughtram/angular-upgrade-app
[ngupgradeslides]: http://thoughtram.io/angular-upgrade-slides/#/
[tses6in60]: https://docs.google.com/presentation/d/1iFsKdrsSkrLt9VUu8nr67iZQJx8Ib4VQY9l3vhzGbIM/edit
[dataarch]: https://docs.google.com/document/d/1olJSkAStuc1gDkaZsMKGbNia5gFGZnqD-1sBybG2Mew/edit#
[library]: https://github.com/ocombe/ng-conf-library
[keynote2]: https://www.youtube.com/watch?v=bSssb9AmiJU