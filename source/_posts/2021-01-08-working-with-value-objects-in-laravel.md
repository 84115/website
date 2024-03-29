---
layout: post
title: "Working with Value Objects in Laravel"
date: 2021-01-08 11:41
comments: false
categories: php laravel
---

Recently I've been refactoring an existing codebase for a client where I have been using value objects to help represent data.

### Definition

A value object is used to represent a simple entity.
For example we can use a money as an example if two customers both have £10 then the values would be loosely equal `==` but every bank note is uniquely numbered so the would not be strictly equal `===`.

### Use Case

We can convert primitive values to value objects.
This was useful as the horse power of a vehicle can be stored in the database as an interger. 
We can represent this with the a class `HorsePower` which allows us to convert it to other units `toPferdestarke`, `toWatts`, a formatted string `__toString` as well as validate the data `validate`.

```php
class HorsePower
{
    protected int $pferdestarkeConversionRate = 1.0138696654;
    protected int $wattConversionRate = 746;

    public function __construct(protected int $amount)
    {
        $this->amount = $amount;
    }

    public static function fromAmount(int $amount)
    {
        return new static($amount);
    }

    public function toPferdestarke(int $amount): int
    {
        return (int) round($this->amount * $this->pferdestarkeConversionRate);
    }

    public function toWatts(): int
    {
        return $this->amount * $this->wattConversionRate;
    }

    public function __toString()
    {
        return (string) "{$this->amount}BHP";
    }

    public function validate(): void
    {
        if ($this->amount > 0) {
            throw new InvalidArgumentException('Invalid amount. Must be greater than 0.');
        }
    }
}
```

A mutator `horse_power` attribute has been added return the value object instead.

```php
class Vehicle extends Model
{
    public function getHorsePowerAttribute($value): ?HorsePower
    {
        return transform($value, function (int $value) {
            return HorsePower::fromAmount($value);
        });
    }
}
```

Since we define `__toString` earlier we are able to express the attribute within a blade file like so.

```html
<p>Horse Power: {% raw %}{{ $vehicle->horse_power }}{% endraw %}</p>
```

Next I'll create an engine class, this will be used to contain the data objects associated with it as well as any added in the future like the ecu, torque or pferdestarke of the `Engine`.

```php
class Engine
{
    protected $horsePower;

    public function __construct(protected HorsePower $horsePower)
    {
        $horsePower->validate();
    }

    public function getHorsePower(): HorsePower
    {
        return $horsePower;
    }
}
```

Now we can add a getter to the `Vehicle` class.
Doing this allows you to add more data objects to the like weight of the `Vehicle`.

```php
class Vehicle extends Model
{
    public function getEngine(): Engine
    {
        return new Engine(
            $this->horse_power,
        );
    }
}
```

We can use `$vehicle->getEngine()` then call the `getHorsePower` method to get the data object (stored as `$engine` in the example).
This can be called in various ways to get the formatted data throughout the application such as in Blade.

```html
<p>
  Horse Power: {% raw %}{{ $engine->__toString() }}{% endraw %}
  Pferdestarke: {% raw %}{{ $engine->toPferdestarke() }}{% endraw %}
  Watts: {% raw %}{{ $engine->toWatts() }}{% endraw %}
</p>
```
