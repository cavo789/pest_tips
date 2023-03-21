# Reuse PHPUnit tests cases without changes

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
