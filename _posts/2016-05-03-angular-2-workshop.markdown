---
layout: post
title:  "Angular2 Workshop"
date:   2016-05-03 18:00:00 -0600
categories: ngconf2016
---

Today I attended a informative and failry intense [Angular2][angular2] workshop put on by [Thoughtram][thoughtram].
It was great fun, fairly fast-paced, and a good introduction to the next version of Angular, though it really just
scratched the surface. Here are a couple of my impressions from that training.

## Style Encapsulation Rocks

One ubiquitous problem that we have is CSS. Specifically we have problems with developers copying CSS to tweak
little bits instead of just tweaking the little bit, and developers allowing changes that are intended to be local
to a directive to bleed into the rest of the system because they do not properly scope their styles.

Since [Angular2][angular2] uses [shadow DOM][shadowDOM], the latter problem is somewhat alleviated due since changesthat truly
should be local to the component will be. Deeper issues of copy-paste styling and failing to apply styles at a
proper level will still need to be handed via pull review, of course, but this will hopefully make the picture
clearer for everyone.

## Typescript

## Simplified Scoping

## Simplified Property and Event Binding

No more overload of directives. [] and () (and {{}}) instead

## Angular CLI

[ngconf]: https://www.ng-conf.org/#/
[angular2]: https://angular.io
[thoughtram]: http://thoughtram.io
[shadowDOM]: http://webcomponents.org/articles/shadow-dom-strategies-in-angular-2/
[typescript}; https://www.typescriptlang.org