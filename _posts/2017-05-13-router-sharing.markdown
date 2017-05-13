---
layout: post
title:  "Routes, Modules, and Angular's AoT Compiler"
date:   2017-05-13
categories: angular routing modules
---

Today I was trying to add a few routes to my application. The routes looks
like this:

```
user-administration                   ## a list of users (admin-only)
user-administration/change-password   ## change my password
user-administration/my-profile        ## update my profile
```

Each of these routes is a different bundle that is dynamically loaded. As a
side note, this means that going to user-administration/change-password causes ttwo modules to be loaded, so I may end up flattening this, but that is a
totally seperate issue. On to the issue at hand.

No matter which route I went to, however, the change-password route kept
coming up. I couldn't figure out why. I was banging my head against a wall
for quite a while trying to make sense of this. Then I started walking
through the various modules involved and all of a sudden it all made sense.
I was including the child modules in the parent module, essentially
causing them to be statically loaded with the parent, while at the same
time being set up to be dynamically loaded in the routing module.

Here is the change-password-routing.module:

```
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { ChangePasswordComponent } from './change-password.component';

const routes: Routes = [
  { path: '', component: ChangePasswordComponent },
  { path: ':id', component: ChangePasswordComponent }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class ChangePasswordRoutingModule { }
```

Then I import that in the change-password.module:

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { MdButtonModule, MdInputModule, MdSnackBarModule } from '@angular/material';
import { RouterModule } from '@angular/router';

import { ChangePasswordComponent } from './change-password.component';
import { ChangePasswordRoutingModule } from './change-password-routing.module';
import { DataModule } from '../../data/data.module';
import { SharedModule } from '../../shared/shared.module';

@NgModule({
  imports: [
    ChangePasswordRoutingModule,

    CommonModule,
    DataModule,
    FormsModule,
    MdButtonModule,
    MdInputModule,
    MdSnackBarModule,
    RouterModule,
    SharedModule
  ],
  declarations: [ChangePasswordComponent]
})
export class ChangePasswordModule { }
```

So far, so good. I created the routes, I import them into the module,
nothing wrong here.

Then I had a look at the routing module for the parent
(user-administration-routing.module):

```
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { UserAdministrationComponent } from './user-administration.component';

const routes: Routes = [
  { path: '', component: UserAdministrationComponent },
  { path: 'change-password', loadChildren: './+change-password/change-password.module#ChangePasswordModule' },
  { path: 'my-profile', loadChildren: './+my-profile/my-profile.module#MyProfileModule' }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class UserAdministrationtRoutingModule { }
```

That looks good too. All of the routes are there. The proper child modules
are loaded on the proper routes. Everything is peachy.

Finally, I looked at the user-administration.module:

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { ChangePasswordModule } from './+change-password/change-password.module';
import { MyProfileModule } from './+my-profile/my-profile.module';
import { DataModule } from '../data/data.module';
import { SharedModule } from '../shared/shared.module';
import { UserAdministrationComponent } from './user-administration.component';
import { UserAdministrationtRoutingModule } from './user-administration-routing.module';

@NgModule({
  declarations: [UserAdministrationComponent],
  imports: [
    ChangePasswordModule,
    MyProfileModule,
    CommonModule,
    DataModule,
    SharedModule,
    UserAdministrationtRoutingModule
  ]
})
export class UserAdministrationModule { }
```

Notice the first two imports? Ooops!! Right there I am loading those two
child modules again, even though I told the router to do it. Also, since
they import their own routing modules and register routes, they were also
registering the base ('') route to be themselves. I am guessing that the
first to register wins, because that is the behavior I was seeing.

Changing the parent module to NOT import the children did the trick:

```
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { DataModule } from '../data/data.module';
import { SharedModule } from '../shared/shared.module';
import { UserAdministrationComponent } from './user-administration.component';
import { UserAdministrationtRoutingModule } from './user-administration-routing.module';

@NgModule({
  declarations: [UserAdministrationComponent],
  imports: [
    CommonModule,
    DataModule,
    SharedModule,
    UserAdministrationtRoutingModule
  ]
})
export class UserAdministrationModule { }

```

Fixed!! And there was much rejoicing.

Lesson learned: remember what strategy you are using for loading the child
modules. If you told the router to do it then don't also load them in the	
parent module. If you do that, you are not going to have a good time.
