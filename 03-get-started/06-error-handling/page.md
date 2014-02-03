---
title: Error Handling
---

Things can go wrong during a Slim Framework application lifecycle. These errors
can be divided into two primary categories.

## 404 Not Found

The "Not Found" error occurs when a user-requested URL cannot be answered by
a matching Slim Framework application route. In this case, the Slim Framework application
will instead invoke and answer with the registered "Not Found" handler.

Slim's default "Not Found" handler will send a `HTTP/1.1 404` response
with a simple repsonse body that reads "Page Not Found". You are encouraged to override
the default "Not Found" handler with your own custom handler.

To register a custom "Not Found" handler, use the Slim Framework application instance's
`notFound()` method. The method argument should be an invokable object or anything
that returns `true` for `is_callable()`.

    <?php
    $app = new \Slim\App();

    $app->notFound(function () use ($app) {
        $app->render('404.html');
    });

    $app->run();

In this example, we register a custom "Not Found" handler that will render a more
appropriate and user-friendly "Page Not Found" template. The "Not Found" handler
is the same as a route callback: it should `echo` content to the current
output buffer. This content will be captured and appended to the Slim Framework
application response object. The response object's status will automatically
be set to `404`.

## 500 System Errors

A "500 System Error" occurs when something on the web server side of things goes wrong.
Your code may throw an unexpected exception, your database may be down, remote APIs
may not respond as you expect them to. The Slim Framework application can help you
anticipate and handle these unexpected errors.

Slim's default "System Error" handler will send a `HTTP/1.1 500` response with a simple
response body that reads "Something went wrong". You are encouraged to override the
default "System Error" handler with your own custom handler.

To register a custom "System Error" handler, use the Slim Framework application instance's
`error` method. The method argument should be an invokable object or anything that
returns `true` for `is_callable()`.

### Debug Mode

It is very important to check the Slim Framework application mode in your error handler!
If the Slim Framework application is in debug mode, you will likely want to print a stacktrace
for debugging purposes.

However, if the Slim Framework application is **NOT** in debug mode, you will likely print
a client-appropriate message like "Something went wrong. We're looking into it." The point being,
it is your responsibility to check the Slim Framework application mode in your custom
error handler and act appropriately. Here is an example:

    <?php
    $app = new \Slim\App();

    $app->error(function (\Exception $e) use ($app) {
        if ($app['mode'] === 'development') {
            // Output exception message
            echo $e->getMessage();

            // Output stacktrace
            debug_print_backtrace();
        } else {
            echo "Something went wrong. We are looking into it.";
            // PRO TIP: Use a third-party logger here (e.g. Monolog)
        }
    });

    $app->run();
