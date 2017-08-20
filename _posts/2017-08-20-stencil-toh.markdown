---
layout: post
title:  "StencilJS and Tour of Heros"
date:   2017-08-20
categories: ionic stencil
---

Disclaimer: I currently work for [Ionic][ionic], but this is not being done
in my official capacity as an employee. Rather, it is being done as a personal
learning experience. Nothing I write here should be taken as "official guidance"
or anything like that. This is just me trying to learn something new in the
way that works best for me.

## Overview

[Ionic][ionic] is working on releasing a new copmonent compiler library called
[Stencil][stenciljs]. In an attempt to familiarize myself with this library and
with creating applications using [Stencil][stenciljs], I have decided to see
how far I can get in re-creating the [Angular][angular] Tour of Hero tutorial
application using only [Stencil][stenciljs] and native browser code [no
frameworks].

This article is being used to document my progress. I will either update it
as I make progress or follow up with other articles. I haven't really worked
that bit out yet.

Each step will be tagged in [my repo][myrepo] for this project.

## Step 1

This step involves just getting the basic application component in place. There
is a single defined hero and an input that allows the user to change the
name of the hero.

I started by cloing the [Stencil Starer Project][stencilstarter] and
reconfiguring the git repo by removing all of the existing git repo information
and creating my own.

Then I added a modifed version of the AppComponent from the Tour of Heros 
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
        <h1 id="title">Tour of Heros</h1>
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


[angular]: https://angular.io
[ionic]: https://ionicframework.com/
[stenciljs]: https://stenciljs.com
[stencilstarter]: https://github.com/ionic-team/stencil-starter 
[myrepo]: https://github.com/kensodemann/stencil-toh
