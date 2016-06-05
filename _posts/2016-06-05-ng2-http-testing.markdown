---
layout: post
title:  "Angular 2 Http Testing"
date:   2016-06-05
categories: Angular2
---

I am working on porting one of my Angular 1.4 applications to Angular 2 RC1. The basic skeleton of the application was
created fairly quickly, but then I hit a minor bump when it came to creating my first service that would do a http
call. The minor bump is the lack of solid documentation on how, exactly, to test that. This post documents what I did.

## Step 1 - Import the Required HTTP Stuff

Similar to Angular 1.x, Angular 2 provides mock HTTP backends for testing purposes. Their use is not as well documented
yet, but they seem to be easier to use. You use them along with some actual http services and interfaces in order to
accomplish your tests. Here are the modules that I have imported.

{% highlight JavaScript %}
import { provide } from '@angular/core';
import { 
  HTTP_PROVIDERS, 
  XHRBackend,
  RequestMethod,
  Response,
  ResponseOptions
} from '@angular/http';
import { MockBackend, MockConnection } from '@angular/http/testing';
{% endhighlight %}

## Step 2 - Provide the HTTP Stuff

We need to set up the HTTP_PROVIDERS and we need to tell the injector to use the MockBackend whenever XHRBackend is requested.
We do this by adding ```HTTP_PROVIDERS``` and ```provide(XHRBackend, { useClass: MockBackend })``` to the ```beforeEachProviders()```
call where the service being tested is already specified.

{% highlight JavaScript %}
beforeEachProviders(() => [
    HTTP_PROVIDERS,
    provide(XHRBackend, { useClass: MockBackend }),
    AuthenticationService
  ]);
{% endhighlight %}

## Step 3 - First Test

My first requirement was simple: POST to the login endpoint. This is a very basic test. A baby step. No testing of the body.
No testing of the response. Let's just see if I can verify that the correct endpoint was hit and that the POST verb was
used. Here is what I came up with:

{% highlight JavaScript %}
  describe('login', () => {
    it('posts to the login endpoint',
      inject([XHRBackend, AuthenticationService], (mockBackend, service: AuthenticationService) => {
        let connection: MockConnection;
        mockBackend.connections.subscribe(c => connection = c);
        service.login();
        expect(connection.request.url).toEqual(`${environment.dataService}/login`);
        expect(connection.request.method).toEqual(RequestMethod.Post);
        connection.mockRespond(new Response(new ResponseOptions({ status: 200 })));
      }));
  });
{% endhighlight %}

In my own words, here is how the test works:

1. Inject both the service and the mock backend
+ Subscribe to connections to the mock backend, within the handler assign the connection to a local variable
+ Call the login
+ Examine the connection's URL and method
+ Send a response via the mock connection

## Step 4 - Next Steps

My next steps are to pass the username and password, and verify the body of the request. After that will come the
response handling. Since the login will return an observable, I will subscribe to it, and then test the results
with success and fail responses. This code will look similar to this:

{% highlight JavaScript %}
service.login().subscribe((res) => {
  expect(...);
});
connection.mockRespond(new Response(new ResponseOptions({ status: 200, ... })));
{% endhighlight %}
