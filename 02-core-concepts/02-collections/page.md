---
title: Collections
---

## What are collections?

Many Slim Framework components share a common feauture: they may contain a collection of
child objects that may be set or retrieved by you, the developer. Examples include:

    * `\Slim\View`
    * `\Slim\Http\Cookies`
    * `\Slim\Http\Headers`

Each collection-like class extends `\Slim\Collection` to ensure it provides the same standardized
methods to manage its collection of child objects. This makes your life as an application developer
much easier, and the Slim Framework simpler to use.

## The interface

The classes listed above implement this interface:

    <?php
    interface CollectionInterface extends \ArrayAccess, \Countable, \IteratorAggregate
    {
        public function set($key, $value);

        public function get($key, $default = null);

        public function replace(array $items);

        public function all();

        public function has($key);

        public function remove($key);

        public function clear();

        public function encrypt(\Slim\Interfaces\CryptInterface $crypt);

        public function decrypt(\Slim\Interfaces\CryptInterface $crypt);
    }
