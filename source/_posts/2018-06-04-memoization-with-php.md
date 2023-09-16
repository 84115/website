---
layout: post
title: "Memoization With PHP"
date: 2018-06-04 12:00
comments: true
categories: php
---

Are you making too many repeated calls to the same resource intensive functions?
Do you need to optimise recursive function calls?
Then memoization could help with that.

### Definition
Memoization is a technique used to increase the speed of recursive or expensive functions by caching the results.
When the same arguments occur again it will return the cache value otherwise it will run the function call.

### Creating the Function
First We'll start by creating a function that returns an anonymous function.

Then create an array that is stored statically to store the caches with `static $caches = []`.

Now we can store the arguments of the function with `$arguments = func_get_args()` and serialize the data to be used as a key `$key = serialize($arguments)`.

And then we'll check if the entry exists `! array_key_exists($key, $caches)` and if not compute then store the value with `$caches[$key] = $function(...$arguments)`.

Finally we can return it via `$caches[$key]`.

```php
function memoize($function)
{
    return function () use ($function) {
        static $caches = [];

        $arguments = func_get_args();
        $key = serialize($arguments);

        if (! array_key_exists($key, $caches)) {
            $caches[$key] = $function(...$arguments);
        }

        return $caches[$key];
    };
}
```

### Demonstration
Here we can create a function that'll we'll pass as an argument to `memoize`.
In this example we'll create a function that double the value passed; for a more real-world example this could be a function call within a recusive loop or a request to an API which would get recalled, like getting user, order, transaction, etc information.

In the example the call we'll make is to `$memoizedDouble(10)` once which will calculate the value.
On subsequent calls of `$memoizedDouble(10)` it will return the cached value in the example.

```php
$double = function ($number) {
    return $number * 2;
};

$memoizedDouble = memoize($add);

// 20
var_dump($memoizedDouble(10));

// 100
var_dump($memoizedDouble(50));

// 20
// Since the exact same arguments have been used
// once already, this will return the cached result.
var_dump($memoizedDouble(10));
```

### In Conclusion
- Memoization is a caching technique, which stores the result of a expensive function call.
- It can be easily implemented to an existing code-base optimise it's performance.
- Memoization can also come in handy when working with 3rd party APIs which will return the same data if called again.