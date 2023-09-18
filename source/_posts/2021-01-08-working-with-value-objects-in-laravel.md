---
layout: post
title: "Working with Value Objects in Laravel"
date: 2021-01-08 12:00
comments: true
categories: php laravel
---

Recently I've been refactoring an existing codebase for a client where I have been using value objects to help represent data.

### Definition

A value object is used to represent a simple entity.
For example we can use a money as an example if two customers both have £10 then the values would be loosely equal `==` but every bank note is uniquely numbered so the would not be strictly equal `===`.

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

```html
<p>Horse Power: {% raw %}{{ $vehicle->horse_power }}{% endraw %}</p>
```

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

```html
<p>
  Horse Power: {% raw %}{{ $engine->__toString() }}{% endraw %}
  Pferdestarke: {% raw %}{{ $engine->toPferdestarke() }}{% endraw %}
  Watts: {% raw %}{{ $engine->toWatts() }}{% endraw %}
</p>
```
