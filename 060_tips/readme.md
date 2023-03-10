# Tips and tricks

## Dump and die

We can use the `dd` method to dump the current expectation value and end the test suite like this:

```php
expect($response)
    ->dd()
    ->toHaveKey('data')
    ->data->toBeEmpty();
```

## PestPhp v2 videos

> [https://freek.dev/2454-pest-v2-see-all-new-amazing-features-in-action](https://freek.dev/2454-pest-v2-see-all-new-amazing-features-in-action)

Very nice ones to discover diamonds like

* `--retry` CLI argument to only re-run previously failed tests,
* `--profile` to see the top 10 slowest tests (so we can try to optimize them),
* `--compact` for a less verbose output
