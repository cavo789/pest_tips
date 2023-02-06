# PestPHP bootstrap

The file `tests/pest.php` can be used to place there global function but we'll also need to update it if, inside our tests files, we need some other classes.

```php
uses(Tests\TestCase::class)->in('Feature');
```

The line above will make `Tests\TestCase` available in all tests in `tests/Feature`. If we need more classes, we can add them:

```php
uses(Tests\TestCase::class,Illuminate\Foundation\Testing\RefreshDatabase::class)->in('Feature');
```

And also in the `test/Unit` folder:

```php
uses(Tests\TestCase::class)->in('Unit');
```
