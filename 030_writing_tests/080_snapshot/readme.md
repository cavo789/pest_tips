# Taking snapshots  

> [🇫🇷 Démonsration du snapshot sur une page web](https://youtu.be/8o1OKFbG854?t=163)

Since Pest 2.9, there is a new feature called `Snapshots`.

The idea is to store a content as a snapshot then compare future run with that snapshot.

A snapshot can be the content of an HTML page, a JSON answer, the content of a file / array, ... everything in fact (for an object; we can serialize it so we can store it too as a snapshot).

```php
it('has a welcome page', function() {
    $response = $this->get('/');
    expect($response)->toMatchSnapshot();
});
```

On the very first run (`vendor/bin/pest`), the snapshot didn't exists yet so it'll be created on disk and the test will be noted as "WARN".

The snapshot will be created in a subdirectory in the `./tests/.pest/snapshots` folder (the subdirectory will match the location of your fired test (f.i. `Feature/ExampleTest/it_has_a_welcome_page.snap`)).

As from the second run, the taken snapshot will then compare with, in the example here above, the HTML content of the homepage. As soon as a difference is noted (like the today date if present on the page), Pest will show it in a diff: the previous string coming from the snapshot and the retrieved, actual, string.

We can update the snapshot by running `vendor/bin/pest --update_snapshots`.