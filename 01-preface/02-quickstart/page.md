---
title: Quick Start
---

## Install the Slim Framework

We recommend you install the Slim Framework using [Composer][composer]. Create a `composer.json` file
in your project working directory with this content:

    {
        "require": {
            "slim/slim": "3.*"
        }
    }

Next, execute this command in your project working directory with your terminal/console application:

    composer install

Composer will download the Slim Framework (and its dependencies) into a `vendors/` directory.

[composer]: https://getcomposer.org/

## Build the Application

Next, create `index.php` in your project working directory with this content:

    <?php
    require 'vendor/autoload.php';

    $app = new \Slim\App();

    $app->get('/hello/:name', function ($name) use ($app) {
        echo "Hello, $name";
    });

    $app->run();

Let's walk through this line by line.

    require 'vendor/autoload.php';

This line activates Composer autoloading. This means you can immediately instantiate any of the Slim Framework
classes and not worry about autoloading.

    $app = new \Slim\App();

This line instantiates a new Slim Framework application. This is the object on which you will define your application
routes and callbacks.

    $app->get('/hello/:name', function ($name) {
        echo "Hello, $name!";
    });

This code defines a new application route, accessible with an HTTP GET request to the URL "/hello/josh". The second
URL segment may be anything you like (e.g "Josh", "Bob", or "Dexter"); it's a "URL route parameter" and its value
will be passed as an argument to the route's callback function. In the example application code above, this route's
callback function accepts one argument, the value of the URL route parameter called ":name", and it `echo`s
a greeting to the current output buffer using the argument value.

    $app->run();

This line _MUST_ be included last so the Slim application will run.

## Serve the Application

We assume you have a web server and a virtual host document root pointed to the same directory that
contains your `index.php` file. We also assume your web server uses URL rewriting to send all
appropriate HTTP requests to the `index.php` file.

If you don't have a web server setup yet, execute this command in your project working directory
with your terminal/console application:

    php -S localhost:4000

This will start an HTTP server listening on port 4000. Open your web browser and navigate to
"http://localhost:4000/hello/there". You will be greeted by your Slim Framework application.
