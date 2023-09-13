---
layout: post
title: "PHP Lambda Functions Approved!"
date: 2019-05-20 12:00
comments: true
categories: php laravel
---

```php
$extended = function ($c) use ($callable, $factory) {
    return $callable($factory($c), $c);
};

// with arrow function
$extended = fn($c) => $callable($factory($c), $c);
```

```php
// Laravel collection example
$users->map(function($user) {
    return $user->first_name . ' ' . $user->last_name;
});

// with arrow function
$users->map(fn($user) => $user->first_name . ' ' . $user->last_name);
```
