# Assertions

> [https://pestphp.com/docs/assertions](https://pestphp.com/docs/assertions)

Assertions comes from PhpUnit and works the same way.

Assertions are accessible through the `$this` object and this because  `tests/pest.php` contains the line below.

```php
uses(Tests\TestCase::class)->in('Feature');
```

So `$this` refers to the `Tests\TestCase` PHPUnit class.
