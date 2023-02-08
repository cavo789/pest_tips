# Tips and tricks

## Dump and die

We can use the `dd` method to dump the current expectation value and end the test suite like this:

```php
expect($response)
    ->dd()
    ->toHaveKey('data')
    ->data->toBeEmpty();
```
