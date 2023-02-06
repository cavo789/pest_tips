# Using datasets

> [https://pestphp.com/docs/datasets](https://pestphp.com/docs/datasets)

We've multiple way to provide data to a function.

Here is [inline](https://pestphp.com/docs/datasets#inline-datasets)

```php
it('has emails', function (string $email) {
    expect($email)->not->toBeEmpty();
})->with([
    'enunomaduro@gmail.com',
    'other@example.com'
]);
```

The dataset is then an array and we can have a multi-dimension array:

```php
it('has emails', function (string $name, string $email) {
    expect($email)->not->toBeEmpty();
})->with([
    ['Nuno', 'enunomaduro@gmail.com'],
    ['Other', 'other@example.com']
]);
```

There is also a way to create a shared dataset which is probably better when the test file becomes big ([https://pestphp.com/docs/datasets#shared-datasets](https://pestphp.com/docs/datasets#shared-datasets)).

