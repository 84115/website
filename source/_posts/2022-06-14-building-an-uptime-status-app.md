---
layout: post
title: "Building an Health Status App"
date: 2022-06-14 12:00
comments: true
categories: linux
---

Lorem ipsum dolor sit amet consectetur adipisicing elit.
Quo enim voluptate assumenda iure sapiente.
Corrupti iusto maiores obcaecati voluptatem, dicta reiciendis voluptatibus fugiat! Quae expedita voluptatibus repellendus nisi ipsam quasi!

### Create Laravel App & Install Packages

```bash
laravel new health

cd health

cp .env.example .env
php artisan key:generate

composer require ukfast/laravel-health-check
composer require guzzlehttp/guzzle

php artisan vendor:publish --provider="UKFast\HealthCheck\HealthCheckServiceProvider" --tag="config"
```

You can register custom middleware to run on requests to the /health endpoint. You can add this to the middleware array in the config/healthcheck.php config file you created using the config above, as shown in the example below:

### Create Your First Health Check

We'll create an example health check for this site.
You should change `J84115` to a namespace for the app or business as well as the class name, `$name`, `$domain` endpoint.

```php
<?php

namespace App\HealthChecks\J84115;

use Illuminate\Support\Facades\Http;
use UKFast\HealthCheck\HealthCheck;

class JamesBallSiteHealthCheck extends HealthCheck
{
    public $name = 'jb-site';
    public $domain = "https://james-ball.co.uk/ping";

    public function status()
    {
        $response = Http::get($this->domain);

        if ($response->successful()) {
            return $this->okay();
        } else {
            return $this->problem("Failed to connect to $this->domain");
        }
    }
}
```

### Register Your check

Open `config/healthcheck.php` and add the HealthCheck class namespace entry to the `checks` index or replace the existing data:

```php
/*
 * List of health checks to run when determining the health
 * of the service
 */
'checks' => [
    App\HealthChecks\J84115\JamesBallSiteHealthCheck::class,
],
```

And if required you can: update the `route-paths.health` value to `json`, this will allow you to access the data as JSON at `{app_url}/json`.

```php
    /**
     * Paths to host the health check and ping endpoints
     */
    'route-paths' => [
        'health' => '/json',
        'ping' => '/ping',
    ],
```

### Create a Page to render the check(s)

Create the following file `resources/views/health.blade.php`:

{% raw %}
```html
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
        <title>Health Checker</title>
    </head>
    <body>
        @foreach ($healths as $name => $health)
            <div>
                <strong>{{ $health->label }}</strong>

                <p style="color: {{ $health->status === 'OK' ? 'green': 'red' }};">
                    {{ $health->status }}
                </p>
            </div>
            <hr>
        @endforeach
    </body>
</html>
```
{% endraw %}

### Pass the Health Check Data to the view

Add the following to `routes/web.php` or a controller that you've created.

```php
use UKFast\HealthCheck\Controllers\HealthCheckController;
```

```php
Route::get('/', function () {
    $response = (new HealthCheckController)->__invoke(app());

    $healths = json_decode($response->content(), true);

    $healths = collect($healths)
        ->map(fn ($value, $key) => (object) ([
            'label' => $key,
            'status' => is_string($value)
                ? $value
                : $value['status'],
        ]));

    return view('health', [
        'healths' => $healths,
    ]);
});
```

### Demo the application

Just start up the Laravel server with:

```bash
php artisan serve
```

The go to [http://localhost:8000](http://localhost:8000) to preview.
