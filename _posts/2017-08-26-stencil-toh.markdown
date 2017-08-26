---
layout: post
title:  "StencilJS and Tour of Heroes - Part 2"
date:   2017-08-26
categories: ionic stencil
---

Disclaimer: I currently work for [Ionic][ionic], but this is not being done
in my official capacity as an employee. Rather, it is being done as a personal
learning experience. Nothing I write here should be taken as "official guidance"
or anything like that. This is just me trying to learn something new in the
way that works best for me.

## Overview

[Ionic][ionic] is working on releasing a new Web Copmonent builder called
[Stencil][stenciljs]. In an attempt to familiarize myself with this library and
with creating applications using [Stencil][stenciljs], I have decided to see
how far I can get in re-creating the [Angular][angular] Tour of Hero tutorial
application using only [Stencil][stenciljs] and native browser code (no
frameworks).

These posts are being used to document my progress.

Each step will be tagged in [my repo][myrepo] for this project.

## Step 2 (continued) - Refactoring and a Fake Save

By the end of my [previous post]({{ site.baseurl }}{% post_url 2017-08-20-stencil-toh %}) I had
created the basic master detail view. There were just a couple of items that needed cleaning up:
changes needed to be reflected in the list, and the selected item needed to show as selected.

The first thing I did was refactor the code a bit. I had the `hero-list` component fetching its
own data. This is making that component way to smart. Most components should be dumb and thus
work reguardless of where their data comes from. So I did the following:

1. moved the fetch of the heroes to the app component, which now owns all data
2. created a heroes property on the hero-list component so it doesn't care where its heroes come from

Next I created a save event on the details which I hadle in the app by replacing the current
heroes data with a copy of itself, in effect simulating a re-read of the data after a change.

```
  @Listen('saveClicked')
  handleSaveClicked() {
    this.heroes = JSON.parse(JSON.stringify(this.heroes));
  }
```

## Failure to Select

Actually, failure to mark as selected...

One last thing I wanted to do is mark the list item with the "selected" class if it was the
selected hero. There is a problem with that, though. Here is what I did:

1. added a property to the `hero-list` compnent to track the currently selected hero
2. added the `@State` decorator to the property to trigger re-rendering on a change
3. added code in the template to set the class

```
export class HeroList {
  @State() selectedHero: Hero;

  @Prop() heroes: Array<Hero>;

  @Event() heroSelected: EventEmitter;

  render() {
    return (
      <div class="hero-list">
        <h2>My Heroes</h2>
        <ul class="heroes">
          {this.heroes.map((hero: Hero) => (<li class={ this.selectedHero === hero && 'selected' } onClick={() => { this.selectedHero = hero; this.heroSelected.emit(hero); }}>
            <span class="badge">{hero.id}</span>{hero.name}</li>))}
        </ul>
      </div>
    );
  }
}
```

Everything mostly works, except in the DOM the class comes out as "0 1 2 3 4 5 6 7" instead of
"selected". I am not sure why. I am also not sure if this is my error or if it is a Stencil
error. I could very easily see that going either way.

There is another bug. This one in my code. Upon re-render due to re-read of the data, the
selected hero will no longer equal any hero in the list, making it look like nothing is
selected. I can fix this (later) by comparing IDs instead of objects. Right now it doesn't
matter though as the class is not being set correctly to begin with.

This is enough adventuring for this morning. Perhaps I will get to it again later. What I
have so far is out on [my repo][myrepo] for the world to see, mistakes and all.

[angular]: https://angular.io
[ionic]: https://ionicframework.com/
[stenciljs]: https://stenciljs.com
[stencilstarter]: https://github.com/ionic-team/stencil-starter 
[myrepo]: https://github.com/kensodemann/stencil-toh
