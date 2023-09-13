---
layout: post
title: "Building an Uptime Status App"
date: 2022-06-14 12:00
comments: true
categories: linux
---

Lorem ipsum dolor sit amet consectetur adipisicing elit.
Quo enim voluptate assumenda iure sapiente.
Corrupti iusto maiores obcaecati voluptatem, dicta reiciendis voluptatibus fugiat! Quae expedita voluptatibus repellendus nisi ipsam quasi!

### Create Laravel App & Install Packages

```bash
laravel new uptime

cd uptime

cp .env.example .env
php artisan key:generate

composer require ukfast/laravel-health-check
```

You can register custom middleware to run on requests to the /health endpoint. You can add this to the middleware array in the config/healthcheck.php config file you created using the config above, as shown in the example below:

### Create Your First Health Check

We'll create an example health check for this site.
You should change `J84115` to a namespace for the app or business as well as the `$name` and `$domain` endpoint.

```php
<?php

namespace App\HealthChecks\J84115;

use UKFast\HealthCheck\HealthCheck;

class JamesBallSiteHealthCheck extends HealthCheck
{
    public $name = 'jb-site';
    public $domain = "https://james-ball.co.uk/ping";

    public function status()
    {
        $curlInit = curl_init($this->domain);

        curl_setopt($curlInit, CURLOPT_CONNECTTIMEOUT, 10);
        curl_setopt($curlInit, CURLOPT_HEADER, true);
        curl_setopt($curlInit, CURLOPT_NOBODY, true);
        curl_setopt($curlInit, CURLOPT_RETURNTRANSFER, true);

        $response = curl_exec($curlInit);

        curl_close($curlInit);

        if ($response) {
            return $this->okay();
        } else {
            return $this->problem("Failed to connect to $this->domain");
        }
    }
}
```

### Register Your check

Open `config/healthcheck.php` and add the HealthCheck class namespace entry to the `checks` index of the file:

```php
/*
 * List of health checks to run when determining the health
 * of the service
 */
'checks' => [
    App\HealthChecks\J84115\JamesBallSiteHealthCheck::class,
],
```

### Create a Page to render the check(s)

Create the following file `resources/views/uptime.blade.php`:

{% raw %}
```html
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
        <title>Health Checker</title>
    </head>
    <body>
        @foreach ($checks as $name => $check)
            <div>
                <strong>{{ $check['label'] }}</strong>

                <p style="color: {{ $check['status'] === 'OK' ? 'green': 'red' }};">
                    {{ $check['status'] }}
                </p>
            </div>
        @endforeach
    </body>
</html>
```
{% endraw %}

### Pass the Health Check Data to the view

Add the following to `routes/web.php` or a controller that you've created.

```php
Route::get('/', function () {
    $response = (new HealthCheckController)->__invoke(app());

    $checks = json_decode($response->content(), true);

    $checks = collect($checks)
        ->map(fn ($value, $key) => [
            'label' => $key,
            'status' => $value['status'],
        ]);

    return view('uptime', [
        'checks' => $checks,
    ]);
});
```

### Demo the application

Just start up the Laravel server with:

```bash
php artisan serve
```

The go to [http://localhost:8000](http://localhost:8000) to preview.
