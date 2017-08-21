---
layout: post
title:  "StencilJS and Tour of Heroes"
date:   2017-08-20
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
application using only [Stencil][stenciljs] and native browser code [no
frameworks].

This article is being used to document my progress. I will either update it
as I make progress or follow up with other articles. I haven't really worked
that bit out yet.

Each step will be tagged in [my repo][myrepo] for this project.

## Step 1 - Basic Application

This step involves just getting the basic application component in place. There
is a single defined hero and an input that allows the user to change the
name of the hero.

I started by cloing the [Stencil Starer Project][stencilstarter] and
reconfiguring the git repo by removing all of the existing git repo information
and creating my own.

Then I added a modifed version of the AppComponent from the Tour of Heroes 
tutorial. It looks like this:

```
import { Component } from '@stencil/core';
import { Hero } from '../../models/hero';

@Component({
  tag: 'stoh-app',
  styleUrl: 'app.scss'
})
export class StohApplication {
  hero: Hero = {
    id: 1,
    name: 'Windstorm'
  };

  componentDidLoad() {
    this.updateSubtitle();
  }

  updateOurHero(event: UIEvent): void {
    this.hero.name = (event.target as HTMLInputElement).value;
    this.updateSubtitle();
  }

  updateSubtitle(): void{
    const subtitle = document.getElementById('subtitle');
    subtitle.innerText = `${this.hero.name} details!`;
  }

  render() {
    return (
      <div>
        <h1 id="title">Tour of Heroes</h1>
        <h2 id="subtitle"></h2>
        <div><label>id: </label>{this.hero.id}</div>
        <div>
          <label>name: </label>
          <input id="hero-name" placeholder="name" value={this.hero.name} onKeyUp={(event: UIEvent) => { this.updateOurHero(event);}} />
        </div>
      </div>
    );
  }
}
```

1. the HTML is supplied via the `render()` function a'la React
2. there is no banana-in-a-box binding for ngModel, instead the `value` is bound
and the key-up event is used to update the data
3. the setting of the subtitle is done via JavaScript

NOTE: I could make the subtitle a component and then use a one-way binding,
but I have not taken this example that far yet.

## Step 2 - Master Detail

At the end of step 1, things were fairly monolithic in that everything was in
the one and only component we had: the application component. This does not
exactly scale well.

The first thing I did for step #2 was to create a child component that shows
the child details and allows us to change the name of the hero.

Here is the updated application component

```
import { Component } from '@stencil/core';
import { Hero } from '../../models/hero';

@Component({
  tag: 'stoh-app',
  styleUrl: 'app.scss'
})
export class StohApplication {
  hero: Hero = {
    id: 1,
    name: 'Windstorm'
  };

  render() {
    return (
      <div>
        <h1 id="title">Tour of Heroes</h1>
        <stoh-hero-details hero={this.hero}></stoh-hero-details>
      </div>
    );
  }
}
```


And here is the new hero details component:

```
import { Component, Prop } from '@stencil/core';

import { Hero } from '../../models/hero';

@Component({
  tag: 'stoh-hero-details'
})
export class HeroDetailsComponent {
  @Prop() hero: Hero;

  updateOurHero(event: UIEvent): void {
    this.hero.name = (event.target as HTMLInputElement).value;
    document.getElementById('subtitle-name').innerText = this.hero.name;
  }

  render() {
    return (
      <div>
        <h2 id="subtitle"><span id="subtitle-name">{this.hero && this.hero.name}</span> details!</h2>
        <div><label>id: </label>{this.hero && this.hero.id}</div>
        <div>
          <label>name: </label>
          <input id="hero-name" placeholder="name" value={this.hero && this.hero.name} onKeyUp={(event: UIEvent) => { this.updateOurHero(event); }} />
        </div>
      </div>
    );
  }
}
```

At this point, we have a much better starting point for adding a list of
heroes and then switching between them. Let's do that now.

Here is the list component that I created. I love the way that the HTML
and JavaScript combine in JSX so you don't have a need for wierd "ngFor" 
style directive or anything. It is just regular code. Also note the event
emitter. Handy...

```
import { Component, Event, EventEmitter } from '@stencil/core';

import { Hero } from '../../models/hero';
import { Heroes } from '../../data/heroes';

@Component({
  tag: 'stoh-hero-list',
  styleUrl: 'hero-list.scss'
})
export class HeroList {
  @Event() heroSelected: EventEmitter;

  render() {
    return (
      <div class="hero-list">
        <h2>My Heroes</h2>
        <ul class="heroes">
          {Heroes.map((hero: Hero) => (<li onClick={() => this.heroSelected.emit(hero)}><span class="badge">{hero.id}</span>{hero.name}</li>))}
        </ul>
      </div>
    );
  }
}
```

Listening for the event in the application component was also super simple:

```
  @Listen('heroSelected')
  handleHeroSelected(event: CustomEvent) {
    this.hero = event.detail as Hero;
  }
```

Step 2 is by no means complete, but it is starting to shape up, and that is
all I have time for this weekend. I'll continue step 2 next weekend or perhaps
some evening this week.


[angular]: https://angular.io
[ionic]: https://ionicframework.com/
[stenciljs]: https://stenciljs.com
[stencilstarter]: https://github.com/ionic-team/stencil-starter 
[myrepo]: https://github.com/kensodemann/stencil-toh
