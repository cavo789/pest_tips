# Introduction about PestPhp

## Files should have the Test suffix

Just like PHPUnit, PestPHP will process every files in folders `tests/Feature` and `tests/Unit` having the `Test` suffix like f.i. `ShoppingBasketTest.php`.

## What means $this in a test?

In our `tests/Pest.php` file, we've this line:

```php
uses(Tests\TestCase::class)->in('Features');
```

In a Pest test, `$this` refers to the PHPUnit `Tests\TestCase` class.

## it or test

PestPHP give us the choice between `it()` and `test()`. *Use the one that best fits your test naming convention, or both. They share the same behavior & syntax.*

Read more: [🇬🇧 https://pestphp.com/docs/writing-tests#api-reference](https://pestphp.com/docs/writing-tests#api-reference)

The result is the same, just how the output is done on the console.
