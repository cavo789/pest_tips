# Tools

## Laravel plugin

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

## Visual Studio Code Addon

* If not yet installad, [🇬🇧 PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client) will allow you to press <kbd>F12</kbd> on a method name (like `toBeTrue`) and jump where the method is implemented,
* [🇬🇧 Better Pest](https://marketplace.visualstudio.com/items?itemName=m1guelpf.better-pest) and
* [🇬🇧 Pest Snippets](https://marketplace.visualstudio.com/items?itemName=dansysanalyst.pest-snippets)

### Better Pest with Docker

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

## Convert from PHPUnit to PestPHP

The repository [🇬🇧 https://github.com/mandisma/pest-converter](https://github.com/mandisma/pest-converter) propose a **PHPUnit to Pest Converter**: PestConverter is a PHP library for converting PHPUnit tests to Pest tests.
