---
layout: post
title: "How to Debug Common Gateway Errors"
date: 2023-09-12 12:00
comments: false
categories: linux nginx php
---

When developing a web application youl commonly run in Gateway errors like`502 Bad Gateway` or `504 Gateway Timeout`.

{% img right /images/posts/bad-gateway.png %}

When PHP cannot process a request that Nginx sends to it, Nginx returns a gateway error. Usually, these errors are not caused by your application, but by some problem that happens before the application handles the request.

### What's The Causes of a Bad Gateway Error?
If PHP-FPM runs into an error then Nginx will render a Gateway error.

This is what will be returned if PHP-FPM runs into an error:

- PHP-FPM is not running (perhaps due to too many error)
- PHP-FPM has reached it's `max_children` limit and cannot process any more requests
- Some sort of PHP error such as a segfault

### Gateway Timeout
When your application receives too much traffic, you might see gateway timeout errors which is when it takes too long to respond to the requests. One reason for this could be that PHP-FPM has reached its `max_children limit` (total number of requests that can be handled at once).

Gateway timeout errors typically occur when your app is handling too much traffic. This can correspond to PHP-FPM `max_children` errors (too many requests that it's configured to handle) but mostly occurs when your database is overloaded and it can't handle additional connections making queries. It takes too much time to return queries.

Another reason for this could be that your application is waiting for a response from a network connection that is taking too long, but the most likely cause is the database being slow.

### Debugging Gateway Errors
To debug the gateway errors the best ways is to check through the logs. It's easiest to start from the top of the server stack then move down like so:

1. Nginx
    - The logs from Nginx usually do not have much useful information, but they might indicate some issues with PHP-FPM not running. For example, if Nginx cannot find the socket file for PHP-FPM, such as `/var/run/php-fpm.sock`, it will show an error in the logs.
2. PHP-FPM
    - The most useful logs are usually the ones from FPM, because FPM is the one that gives us the error from the gateway. Sometimes you will see an error that says you have reached the limit of `max_children`.
3. Server resource usage
    - `top` can be used to check your server resource usage. `df -h` can be used to check if the disk is low on space.
4. Application logs
    - If you've still not found the cause of the error there could database error, error in your code, a timeout or another issue.
