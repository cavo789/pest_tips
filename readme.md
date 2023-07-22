<!-- This file has been generated by the concat.sh script. -->
<!-- Don't modify this file manually (you'll loose your changes) -->
<!-- but run the tool once more -->
<!-- Last refresh date: Saturday, July 22, 2023, 09:53:24 -->

# Write PHP Unit tests using PestPHP

![Banner](./banner.svg)

> [https://pestphp.com/](https://pestphp.com/)

PestPHP is a wrapper around PhpUnit so, for instance, every command line arguments supported by PhpUnit can be used for PestPHP.

<!-- table-of-contents - start -->
* [Introduction](#introduction)
* [Installation](#installation)
* [Writing tests](#writing-tests)
  * [Introduction about PestPhp](#introduction-about-pestphp)
    * [Files should have the Test suffix](#files-should-have-the-test-suffix)
    * [What means $this in a test?](#what-means-this-in-a-test)
    * [it or test](#it-or-test)
  * [Our first tests](#our-first-tests)
    * [Autocomplete](#autocomplete)
    * [Difference between toBe and toEqual](#difference-between-tobe-and-toequal)
  * [Assertions](#assertions)
  * [Expectations](#expectations)
  * [Using datasets](#using-datasets)
  * [Reuse PHPUnit tests cases without changes](#reuse-phpunit-tests-cases-without-changes)
  * [Architectural tests](#architectural-tests)
  * [Taking snapshots](#taking-snapshots)  
* [Write global functions](#write-global-functions)
* [PestPHP bootstrap](#pestphp-bootstrap)
* [Tips and tricks](#tips-and-tricks)
  * [Dump and die](#dump-and-die)
  * [PestPhp v2 videos](#pestphp-v2-videos)
* [Convert from PHPUnit](#convert-from-phpunit)
* [Tools](#tools)
  * [Laravel plugin](#laravel-plugin)
  * [Visual Studio Code Addon](#visual-studio-code-addon)
    * [Better Pest with Docker](#better-pest-with-docker)
  * [Convert from PHPUnit to PestPHP](#convert-from-phpunit-to-pestphp)
* [Links](#links)
  * [Videos](#videos)<!-- table-of-contents - end -->

## Introduction

## Installation

> [🇬🇧 https://pestphp.com/docs/installation#installation](https://pestphp.com/docs/installation#installation)

```bash
composer require pestphp/pest --dev --with-all-dependencies

composer require pestphp/pest-plugin-laravel --dev
php artisan pest:install

./vendor/bin/pest --init
```

From now, we can run `./vendor/bin/pest` to run our PestPHP tests.

## Writing tests

### Introduction about PestPhp

#### Files should have the Test suffix

Just like PHPUnit, PestPHP will process every files in folders `tests/Feature` and `tests/Unit` having the `Test` suffix like f.i. `ShoppingBasketTest.php`.

#### What means $this in a test?

In our `tests/Pest.php` file, we've this line:

```php
uses(Tests\TestCase::class)->in('Features');
```

In a Pest test, `$this` refers to the PHPUnit `Tests\TestCase` class.

#### it or test

PestPHP give us the choice between `it()` and `test()`. *Use the one that best fits your test naming convention, or both. They share the same behavior & syntax.*

Read more: [🇬🇧 https://pestphp.com/docs/writing-tests#api-reference](https://pestphp.com/docs/writing-tests#api-reference)

The result is the same, just how the output is done on the console.

### Our first tests

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

#### Autocomplete

![Auto complete](./030_writing_tests/020_first_tests/010_autocomplete/images/autocomplete.png)

Make sure to install and enable [PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client) and enjoy the auto-complete feature of vscode.

#### Difference between toBe and toEqual

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

### Assertions

> [🇬🇧 https://pestphp.com/docs/assertions](https://pestphp.com/docs/assertions)

Assertions comes from PhpUnit and works the same way.

Assertions are accessible through the `$this` object and this because  `tests/pest.php` contains the line below.

```php
uses(Tests\TestCase::class)->in('Feature');
```

So `$this` refers to the `Tests\TestCase` PHPUnit class.

### Expectations

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

### Using datasets

> [🇬🇧 https://pestphp.com/docs/datasets](https://pestphp.com/docs/datasets)

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

### Reuse PHPUnit tests cases without changes

This is damned simple, we just need to add `/** @test */` as doc-block before the test scenario.

For instance:

```php
<?php

declare(strict_types=1);

namespace Tests\Feature;

use Tests\TestCase;

class VisitLoginPageTest extends TestWebCase
{
    /** @test */
    public function test_we_should_see_fields_email_and_password()
    {
        $this->response->assertSee('id="password"', false);
        $this->response->assertSee('id="email"', false);
    }
}
```

And from now that test can be fired using `./vendor/bin/pest`.

### Architectural tests

> [🇬🇧 https://pestphp.com/docs/arch-testing](https://pestphp.com/docs/arch-testing)

> [🇫🇷 Tests d'architecture avec Pest 2.9](https://youtu.be/8o1OKFbG854)

Using PestPHP v2, we can ensure some architectural consistencies like not using validations in a controller (using `$request->validate(...)`) but forcing to use the Form request control classes.

The architectural plugin will not help to fire unit tests but will scan the project and will ensure some rules are followed.

Architectural tests can be:

```php
test('controllers')
    ->expect('App\Http\Controllers')
    ->not->toUse('Illuminate\Http\Request');

// Models can only be used in a repository
test('models')
    ->expect('App\Models')
    ->toOnlyBeUsedOn('App\Repositories')
    ->toOnlyUse('Illuminate\Database');

test('repositories')
    ->expect('App\Repositories')
    ->toOnlyBeUsedOn('App\Controllers')
    ->toOnlyUse('App\Models');

test('globals')
    ->expect(['dd','dump','var_dump'])
    ->not->toBeUsed();

test('facades')
    ->expect('Illuminate\Support\Facades')
    ->not->toBeUsed()
    ->ignoring('App\Providers');
```

This part can be seen on video [https://youtu.be/9EGPo_enEc8?t=1021](https://youtu.be/9EGPo_enEc8?t=1021)

Since Pest v2.9, we can also check if a class is final:

```php
test('controllers')
    ->expect('App\Http\Controllers')
    ->toUseStrictTypes()
    ->toHaveSuffix('Controller') // or toHavePreffix, ...
    ->toBeReadonly()
    ->toBeClasses() // or toBeInterfaces, toBeTraits, ...
    ->classes->toBeFinal() // 🌶
    ->classes->toExtendNothing() // or toExtend(Controller::class),
    ->classes->toImplementNothing() // or toImplement(ShouldQueue::class),
```

### Taking snapshots  

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

## Write global functions

We can write our own custom functions in the `tests/Pest.php` file.

## PestPHP bootstrap

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

## Tips and tricks

### Dump and die

We can use the `dd` method to dump the current expectation value and end the test suite like this:

```php
expect($response)
    ->dd()
    ->toHaveKey('data')
    ->data->toBeEmpty();
```

### PestPhp v2 videos

> [🇬🇧 https://freek.dev/2454-pest-v2-see-all-new-amazing-features-in-action](https://freek.dev/2454-pest-v2-see-all-new-amazing-features-in-action)

Very nice ones to discover diamonds like

* `--retry` CLI argument to only re-run previously failed tests,
* `--profile` to see the top 10 slowest tests (so we can try to optimize them),
* `--compact` for a less verbose output

## Convert from PHPUnit

Introduced in Pest v2.9, there is a tool to convert from PHPUnit to Pest.

See [🇬🇧 https://pestphp.com/docs/pest-spicy-summer-release#content-drift-plugin](https://pestphp.com/docs/pest-spicy-summer-release#content-drift-plugin)

Note: Rector has also a tool: [🇬🇧 https://github.com/rectorphp/rector-pest](https://github.com/rectorphp/rector-pest)

For example, the code below 

```php
<?php
 
namespace Tests\Unit;
 
use PHPUnit\Framework\TestCase;
 
class ExampleTest extends TestCase
{
    public function test_that_true_is_true(): void
    {
        $this->assertTrue(true);
    }
}
```

will be converted to 

```php
test('true is true', function () {
    expect(true)->toBeTrue();
});
```

## Tools

### Laravel plugin

> [🇬🇧 Laravel Pest plugin](https://pestphp.com/docs/plugins/laravel)

Install the plugin like this: `composer require pestphp/pest-plugin-laravel --dev`

Then some new artisan commands will be available:

```bash
php artisan | grep pest
 pest
  pest:dataset           Create a new dataset file
  pest:install           Creates Pest resources in your current PHPUnit test suite
  pest:test              Create a new test file
```

### Visual Studio Code Addon

* If not yet installad, [🇬🇧 PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client) will allow you to press <kbd>F12</kbd> on a method name (like `toBeTrue`) and jump where the method is implemented,
* [🇬🇧 Better Pest](https://marketplace.visualstudio.com/items?itemName=m1guelpf.better-pest) and
* [🇬🇧 Pest Snippets](https://marketplace.visualstudio.com/items?itemName=dansysanalyst.pest-snippets)

#### Better Pest with Docker

If you're using Docker, think to add the next lines in your `.vscode/settings.json` configuration file:

```json
{
    "better-pest.docker.enable": true,
    "better-pest.docker.command": "docker compose exec -u root:root app",
    "better-pest.docker.paths": {
        "/your/local/path": "/your/remote/path"
    }
}
```

Think to adjust the name of your container (`app` here) and paths:

* `/your/local/path` is where your repository is stored, on your host machine,
* `/your/remote/path` is the path in your container, probably `/var/www/html`.

Now, just open any PestPhp file and press <kbd>CTRL</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> to open the Command Palette. Start to type `Better Pest` and select the desired option (like `Better Pest: run` for running the file).

### Convert from PHPUnit to PestPHP

The repository [🇬🇧 https://github.com/mandisma/pest-converter](https://github.com/mandisma/pest-converter) propose a **PHPUnit to Pest Converter**: PestConverter is a PHP library for converting PHPUnit tests to Pest tests.

## Links

### Videos

* [🇬🇧 Laracast - PestPHP](https://laracasts.com/series/jeffreys-larabits/episodes/30)
* [🇬🇧 Laracast - Pest from Scratch](https://laracasts.com/series/pest-from-scratch)
* [🇬🇧 Laracon IN 2023: the future of PEST](https://youtu.be/9EGPo_enEc8)
