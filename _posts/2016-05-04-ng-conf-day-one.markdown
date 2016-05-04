---
layout: post
title:  "ng-conf 2016 - Day One"
date:   2016-05-04 09:00:00 -0700
categories: ngconf2016
---

## Keynote Addresses

Angular usage continues to increase. As of October 2015, there were over 1.1 million developers using version 1.x.
That number has grown to over 1.3 million developers today. In addition, roughly 360k developers are currently
using [Angular 2][angular2], even though it has just gotten out of Beta and is now in RC1.

The overall goals for Angular have changed from 1.x to version 2. For Angular 1.x, the main goal was to simply
create an easier way to build and test web applications. The goals for [Angular 2][angular2], however have
expanded to providing a complete platform with lazy loading, decorators, zones, etc.

Several interesting items were pointed out in the keynote address. Angular 2:

* has moved from Beta into RC1
* is now smaller than Angular 1.x, weighing in currently at 45k
* has a much improved compiler with off line building
* has faster change detection
* has a faster rendering engine (5x faster than Angular 1)
* can render to the browser and on the server
* is starting to invest in mobile and has its own [mobile toolkit][mobile]

Several interesting [Angular 2][angular2] related projects or features were also higlighted, including:

* Angular Universal allows us to run angular on the server. In this case, rendering is done on server and
then sent to the client device. Angular Universal is currently avaliable in node.js and ASP.NET.
* Angular CLI provides tools for scaffolding, building, and running applications
* Serveral laguage services for IDEs have been created
* Augury is a Chrome plug-in for debugging and application analysis
* ngUpgrade allows us to mix Angular 2 services and components into angular 1 applications

[Angular 2][angular2] is designed around making code easier to structure and to understand. As an example, Angular 1
had 43 directives such as ngBind, ngShow, ngHide, ngClick, etc. These are now replace by two constructs: [] - bind to
property and () - bind to event. The encapsulation brought on by the use of components and the shadow DOM are
also examples of this.

It is important for code to follow a consistent style and to follow specific convensions. This is especially 
beneficial when onboarding new developers. [Angular 2][angular2] has a new [style guide][angular2styleguide]
that helps facilitate best practices and best styles. There is also a command line node.js tool called codelyzer
that will analyze Typescript code enforcing best practices and styles.

Finally, some interesting integrations with other libraries were mentioned:

* UI Components - [Angular Material 2][material] & Friends (ng2-bootstrap is available, for example)
* Installed Mobile - [Ionic][ionic], [NativeScript][nativescript], ReactNative
* Installed Desktop - with Electron we can now partially move angular into the node.JS portion

## Shadow DOM (Style)

A continuous problem in the development of web applications is CSS bleed, where styles meant to apply in very 
specific instances bleed into other uses of these same constructs or classes. The only good way around this problem
is through proper class disipline and some very specific classing, which often results in very long and odd class
names.

[Shadow DOM][shadowdom] helps to alleviate these problems by isolating the various components in the application.
The problem is that not all browsers support native shadow DOM. [Angular 2][angular2] handles this by supporting
three different modes of shadow DOM: none, emulated, and native. The default is emulated, which allows for
shadow DOM to be used on any modern-ish browser by shimming the classes, styles, and components at compile time.

One note of caution: you can set the style on a component by component basis, but doing so could lead to problems
if you are not careful.

## Typescript

[Typescript][typescript] is build on top of ES5, ES2016, and ES2016, meaning that it is a superset of JavaScript.
You can actually take your native ES5 code and plug it in to a Typescript compiler and expect the compiler to find
issues with your code. Typescript adds several key features on top of JavaScript: Type support, Interfaces, and
Generics. These features allow for better tooling support which in turn allows for fewer mistakes in coding.

### Type Support

Type Support includes the use of [basic core types][basictypes], but also user defined types (via interfaces) and
andvanced types such as union types and intersception types:

{% highlight JavaScript %}
// union type, accessories is either a string or an array of strings
let accessories: string | string[];

// intersception type, buyer is a person and a customer
let buyer: person & customer;
{% endhighlight %}

### Interfaces

Interfaces are a code contract that must be met in order for a class or object to be valid. These interfaces are
very similar to interfaces in .NET, but are not _exactly_ the same. For example, variables can be declared based
on the interface, whereas in many other languages a concrete type must first be created that implements that 
interface.
 
Here is an example:

{% highlight JavaScript %}
interface ICustomer {
  firstName: string;
  lastName: string;
  age?: number;
}

// This must not have extra properties not defined by the interface, and we are saying
// that this IS an ICustomer
let person: ICustomer = {
  firstName: 'Billy',
  lastName: 'Robertson'
};

// Here the extra properties are allowed because the object meets the interface,
// that is, the object implements the interface
function foo(c: ICustomer) {...}

let customer = {
  firstName: 'Billy',
  lastName: 'Robertson',
  gender: 'boy type person'
};

foo(customer);
{% endhighlight %}

### Generics

Generics are code templates and they work very similarily to code templates in other languages such as C#.
This allows for better code reuse.

{% highlight JavaScript %}
export class List<T> {
  add(item: T) {}
}

let customers = new List<ICustomer>();
{% endhighlight %}

### Tooling Support

Types, interfaces, and generics allow IDE developers to provide better error highlighting and code completion,
allowing for fewer coding mistakes and allowing mistakes that are made to be found more quickely.

## Components and Angular 1.5

Components are the future of web apps, and Angular 1.5 supports components.

Presentation components (dumb, stateless)
 
Business components (smart, do not provide UI, make state changes, render other components)

View components specialized versions of business components that know about the router, can create components dynamically.

{% highlight JavaScript %}
myModule.component('myComponent', {
  template:
  bindings:
  controller: 
  controllerAs: 'controller'
});
{% endhighlight %}

Angular 1.5 components also have lifecylce hooks that are very similar to the lifecycle hooks in [Angular 2][angular2].

This gets us one step closer to _finally_ killing $scope in Angular 1.5.

$componentController in ngMock aids with testing components by creating instances of the named component's controller.

The component router has been backported so it can be used in Angular 1.5

## Augury (formerly Bataragle)

[Augury][augury] is more than just a tool to find bugs, but an aid in determining your application's structure at
runtime. This tool allows you to display component relationships, dependency hierarchies, and router structures.
It also allows you to easily examine the Typescript source for components and to modify the editable properties
of components.

This tool is a Chrome plug-in.

## Progressive Web Applications

The typical native mobile application model has some obvious flaws. The bigget being duplication of effort
and the fact that the applications need to be installed on the devices, generally through the stores, and this
is a high friction method of deployment.

Web applications, on the other hand are searchable, sharable, and very low friction. Yet this does not translate
to mobile very well since there are things about native applications that are better than web applications:
performance is better, re-engagement is a plus (push notifications, installed on the home screen, etc), and API
access (camera, geo-location, etc).

Progressive Web Applications are starting to come through as providing the best of both worlds on this.
Progressive Web Apps are essentially best practices and techniques based on emerging standards. The big four
emerging capabilities are: instant loading, offline access, installablity, and notifications.

The mobile team believes this is going to be the future of the web and of mobile application development.

The application gets instant loading via App Shell and Angular Universal to pre-genetate the App Shell component
in the index.html Offline access is handled via Service Worker. App Shell + Service Worker give us many of the
advantages of native applications. These applications are made installable via the Web Application Manifest.
However, iOS hasn't quite evolved enough to make it
completely usable yet.

All of these are packaged together via the [Angular Mobile Tookit][mobile], which is still in Alpha at this time.
This is something worth keeping an eye on in my opinion.

[angular15comp]: https://docs.angularjs.org/api/ng/provider/$compileProvider#component
[angular2styleguide]: https://angular.io/styleguide
[angular2]: https://www.angular.io
[mobile]: http://mobile.angular.io
[nativescript]: https://www.nativescript.org
[typescript]: https://www.typescriptlang.org
[basictypes]: https://www.typescriptlang.org/docs/handbook/basic-types.html
[shadowdom]: http://webcomponents.org/articles/shadow-dom-strategies-in-angular-2/
[augury]: https://augury.angular.io
[material]: https://material.angular.io
[ionic]: http://ionicframework.com/docs/v2
[nativescript]: https://www.nativescript.org