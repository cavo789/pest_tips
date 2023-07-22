# Architectural tests

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
