---
layout: post
title: "MJML to HTML using PHP"
date: 2023-08-29 12:00
comments: false
categories: laravel
---

If you want to create responsive email templates with ease, you can use MJML, a markup language that simplifies the process. MJML by Spatie uses semantic tags that work well even in Outlook.

First install the composer package:

```bash
composer require spatie/mjml-php
```

Then you can render a simple hello world message like so:

```php
use Spatie\Mjml\Mjml;
 
$markup = <<<'MJML'
    <mjml>
      <mj-body>
        <mj-section>
          <mj-column>
            <mj-text>Hello World</mj-text>
          </mj-column>
        </mj-section>
      </mj-body>
    </mjml>
MJML;
 
$body = Mjml::new()->toHtml($markup);
```

The MJML PHP package and documentation can be found via GitHub at <a target="_blank" href="https://github.com/spatie/mjml-php">spatie/mjml-php</a>.
