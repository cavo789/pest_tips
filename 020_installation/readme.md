# Installation

> [https://pestphp.com/docs/installation#installation](https://pestphp.com/docs/installation#installation)

```bash
composer require pestphp/pest --dev --with-all-dependencies

composer require pestphp/pest-plugin-laravel --dev
php artisan pest:install

./vendor/bin/pest --init
```

From now, we can run `./vendor/bin/pest` to run our PestPHP tests.
