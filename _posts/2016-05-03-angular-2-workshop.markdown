---
layout: post
title:  "Angular 2 Workshop"
date:   2016-05-03
categories: ngconf2016
---

Today I attended a informative and failry intense [Angular 2][angular2] [workshop][slides] put on by [Thoughtram][thoughtram].
It was great fun, fairly fast-paced, and a good introduction to the next version of Angular, though it really just
scratched the surface. Here are a couple of my impressions from that [training][slides].

Here are the official git repositories for the training:

* [Starter][starter] - the starter will fail to run, that is expected, fixing it is Execise #1
* [Exercise Descriptions][descriptions]
* [Exercise Solutions][solutions]

Also, I have [my own reponsitory][mine] with commits for each step.

## Style Encapsulation Rocks

One ubiquitous problem that we have is CSS. Specifically we have problems with developers copying CSS to tweak
little bits instead of just tweaking the little bit, and developers allowing changes that are intended to be local
to a directive to bleed into the rest of the system because they do not properly scope their styles.

Since [Angular 2][angular2] uses [shadow DOM][shadowDOM], the latter problem is somewhat alleviated due since changesthat truly
should be local to the component will be. Deeper issues of copy-paste styling and failing to apply styles at a
proper level will still need to be handed via pull review, of course, but this will hopefully make the picture
clearer for everyone.

## Typescript

I am really super excited about using [TypeScript][typescript] by default. Up until now, we have been using strictly
ES5 for our development. ES5 is great, but it is missing some really cool features of modern languages such as 
classes, interfaces, and generics. TypeScript adds all of these. On top of that, TypeScript is a super-set of
JavaScript, so any valid ES5 is also valid TypeScript.

I know we could be using TypeScript today, but it would complicate our tooling. With [Angular 2][angular2],
it is part of the tooling and a more natural fit.

## Simplified Scoping

Angular 1 had $scope. Thankfully, $scope is dead, dead, dead. Each component has its own scope, you know what the
scope is, you never have to worry about anything having to do with a hierarchy of $scopes. Life is good.

## Simplified Property and Event Binding

Angular 1 has a lot of directives, as in 43 speciallized directives such as ng-show, ng-hide, ng-bind, ng-click,
etc. In [Angular 2][angular2], that all goes away and is replaced by [] for property bindings and () for event
bindings. This greatly simplifies the amount of crap a developer needs to know in order to be productive.

## Angular CLI

The [Angular CLI][ngcli] scafolds the project, puts stuff where it belongs, creates shells for new components for
you, etc. It will take a little more playing with this to make sure that it will do what we need it to (if it doesn't,
it is open source so we can submit pull requests and/or tweak it ourself (I would highly discourage the latter)).
It may or may not be configurable for our needs.

Bottom line, it made working with the tutorial code a joy, but we will need to determine how flexible it is for
use on larger projects like ours.

[ngconf]: https://www.ng-conf.org/#/
[angular2]: https://angular.io
[thoughtram]: http://thoughtram.io
[shadowDOM]: http://webcomponents.org/articles/shadow-dom-strategies-in-angular-2/
[typescript]: https://www.typescriptlang.org
[ngcli]: https://cli.angular.io
[starter]: https://github.com/thoughtram/angular2-master-class-starter
[slides]: http://thoughtram.io/angular2-master-class-jump-start-slides/#/
[descriptions]: https://github.com/thoughtram/angular2-master-class-jump-start-exercise-descriptions
[solutions]: https://github.com/thoughtram/angular2-master-class-jump-start-solutions
[mine]: https://github.com/kensodemann/angular2-master-class