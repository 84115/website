---
layout: post
title: "Laravel Impersonate"
date: 2023-03-22 22:34
comments: false
categories: php laravel
---

I've recently developed a Laravel Package to temporarily login as other users when signed in as an admin, [Download here](https://github.com/84115/Laravel-Impersonate).

## Usage
- You can login to another user through `{app_url}/impersonate/login/{user_id}`.
- And you can end the session with `{app_url}/impersonate/logout`.

## Install Package
```bash
composer require j84115/impersonate
```

## Add Sevice Provider
Add the Package to `config/app.php`

```php
J84115\Impersonate\ImpersonateServiceProvider::class,
```

## Add Interface To User
Add the Interface to your User Model. Typically `app/Models/User.php`.

```php
use J84115\Impersonate\Interfaces\ImpersonateUser;
```

Implement the interface.

```php
class User extends Authenticatable implements ImpersonateUser
```

Then add your conditions for who can impersonate a user.

```php
    public function impersonator(): bool
    {
        return $this->role === 'admin';
    }

    public function impersonatable(): bool
    {
        return $this->email !== 'admin';
    }
```

## Routing
Add the following macro to your routes. Typically guarded with auth Middleware in `routes/web.php`.

```php
Route::impersonate();
```
