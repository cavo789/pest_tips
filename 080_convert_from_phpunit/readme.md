# Convert from PHPUnit

Introduced in Pest v2.9, there is a tool to convert from PHPUnit to Pest.

See [https://pestphp.com/docs/pest-spicy-summer-release#content-drift-plugin](https://pestphp.com/docs/pest-spicy-summer-release#content-drift-plugin)

Note: Rector has also a tool: [https://github.com/rectorphp/rector-pest](https://github.com/rectorphp/rector-pest)

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
