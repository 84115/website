---
layout: post
title: "Working with Value Objects in Laravel"
date: 2021-01-08 12:00
comments: true
categories: php laravel
---

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
<p>Horse Power: {{ $vehicle->horse_power }}</p>
```

```php
class EngineServiceState
{
    protected $horsePower;

    public function __construct(protected HorsePower $horsePower)
    {
        $this->isHorsePowerValid($horsePower);
    }

    public function getHorsePower(): HorsePower
    {
        return $horsePower;
    }

    protected function isHorsePowerValid(HorsePower $horsePower): void
    {
        if ($horsePower->amount > 0) {
            throw new InvalidArgumentException('Invalid amount. Must be greater than 0.');
        }
    }
}
```

```php
class Vehicle extends Model
{
    public function getEngineServiceState(): EngineServiceState
    {
        return new EngineServiceState(
            $this->horse_power,
        );
    }
}
```

```html
<p>
  Horse Power: {{ $horsePower->__toString() }}
  Pferdestarke: {{ $horsePower->toPferdestarke() }}
  Watts: {{ $horsePower->toWatts() }}
</p>
```
