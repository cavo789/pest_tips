# Expectations

> [🇬🇧 https://pestphp.com/docs/expectations](https://pestphp.com/docs/expectations)

*In addition to assertions, Pest offers you a set of expectations. These functions let you test your values against certain conditions. This API is inspired by Jest. Expectations also allow you to write your tests like you would a natural sentence*

Assertions and expectations can be used in PestPHP tests files but ... expectations are more explicits and intuitive.


```php
<?php

test('assert true is true', function () {
    // These two lines do exactly the same. Keep just one...
    $this->assertTrue(true);
    expect(true)->toBeTrue();
});
