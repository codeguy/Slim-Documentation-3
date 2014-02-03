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

Coming soon...
