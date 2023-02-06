# Difference between toBe and toEqual

```php
<?php

test('assert count is correct', function () {
    expect(2 + 2)->toBe(4);       // Will be true
    expect(2 + 2)->toBe('4');     // Will NOT be true

    expect(2 + 2)->toEqual(4);    // Will be true
    expect(2 + 2)->toEqual('4');  // Will be true 
});
```

`toBe` will be more strict i.e. will check both the value and the data type when, `toBe` will just check the value.
