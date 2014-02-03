---
title: Request Lifecycle
---

## Overview

When you run a Slim Framework application, a lot of things happen beneath
Slim's simple, user-friendly public interface. For the purposes of this documentation,
I will use this example Slim Framework application:

    <?php
    $app = new \Slim\App();

    $app->get('/foo', function () {
        echo "Bar";
    });

    $app->run();

## 1. Web Server URL Rewriting

When a visitor enters your application's domain name (e.g. "myapp.example.com") into his web browser
(or alternative HTTP client), the web browser sends an HTTP request to the server
identified by your domain name's IP address. The server receives the HTTP request and delegates
the HTTP request handling to a web server (e.g. Apache). The web server rewrites
_all appropriate URLs_ it receives so they are re-routed to a single PHP file. This PHP file contains
your Slim Framework application.

## 2. Application Instantiation

The PHP file that receives all appropriate inbound HTTP requests typically first instantiates
a new Slim Framework application. During instantiation, the Slim Framework specifies how its
default internal objects (request, response, view, and so forth) are created when requested
elsewhere in the application. Note: none of these objects are actually instantiated yet.

Because no internal objects are instantiated in the application constructor, you (the developer)
can easily override any or all default internal object implementations using dependency injection.
More on dependency injection later...

## 3. Route Definitions

After the Slim Framework application is created, you typically define your application routes
using any of these public methods on the `\Slim\App` class:

* `get()`
* `post()`
* `put()`
* `delete()`
* `options()`
* `patch()`

Isn't it convenient how each method corresponds to an HTTP verb ;-) When a route is defined, it
is stored inside the Slim Framework application to be invoked later if it matches the current
HTTP request. Note: still nothing has really happened just yet.

## 4. Run the Application

### Finding a matching route

Now the fun begins! When you invoke the Slim Framework application instance's `run()`
method, Slim iterates your defined routes and find the one route (if any) that matches
the current HTTP request's method and URL. If a matching route is found, its assigned
callback is invoked. If a matching route is not found, the application's "Not Found"
handler is invoked instead.

Each Slim Framework application route consists of a URL pattern and a callback. The URL pattern
is matched against the current HTTP request. The callback can be anything that is invokable
or returns `true` for `is_callable()`.

### Prepare the Environment, Request, and Response objects

Immediately before the Slim Framework application searches for matching routes, it instantiates
Environment, Request, and Response objects. The Environment object parses the current
`$_SERVER` superglobal array for the current HTTP request details (method, headers, etc.).
The Request object contains the Environment object and references the Environmenet object
to inspect the raw HTTP request details.

### Invoking a matching route

It is the callback's responsibility to `echo` content to the current output buffer.
The output buffer contents are captured and appended to the Slim Framework application's
response object immediately after the route callback is invoked.

### Finalizing the HTTP response

After the matching route callback is invoked and the output buffer content appended to the
response object, Slim performs a few clean up tasks such as:

* Saving flash messages to the session
* Encrypting and saving the session
* Setting the HTTP response status, headers, and body
* Sending the HTTP response back to the web server

There is, of course, more going on than the simplistic explanations above. But this is an
accurate (albeit simplified) explanation of the Slim Framework application request
lifecylce. Explore this documentation for more information.
