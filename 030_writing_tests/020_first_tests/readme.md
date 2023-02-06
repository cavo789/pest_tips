# Our first tests

Create a file like `tests/Feature/MyFirstTest.php` with this content:

```php
<?php

test('assert true is true', function () {
    expect(true)->toBeTrue();
});

test('assert false is not true', function () {
    expect(false)->not->toBeTrue();   // we can also write `not()->`
});
```

This illustrate that PestPhp start with a `expect` verb and some method like `toBeTrue()`.  Methods can be negated using `not->` ([https://pestphp.com/docs/expectations#expect-not](https://pestphp.com/docs/expectations#expect-not)).

Running our test can be simply done using `./vendor/bin/pest tests/Feature/MyFirstTest.php` and here is the result:

```text
   PASS  Tests\Feature\MyFirstTest
  ✓ assert true is true
  ✓ assert false is not true

  Tests:  2 passed
  Time:   0.08s
```
