<a id="docs-introduction"></a>
## Introduction

**Simple QrCode** is an easy-to-use wrapper for the Laravel framework,  
based on the excellent [Bacon/BaconQrCode](https://github.com/Bacon/BaconQrCode) library.  

Originally created by *Simple Software LLC (2014–2020)*,  
this package is now maintained by **DevsApps LTD** to ensure compatibility with modern Laravel and PHP versions.

![Example 1](docs/imgs/example-1.png) ![Example 2](docs/imgs/example-2.png)

---

<a id="docs-upgrade"></a>
## Upgrade Guide

Upgrade from v2 or v3 by requiring version `~4` in your `composer.json`.

You **must** install the `imagick` PHP extension if you plan on using the `png` format.

#### v4

- The `generate` method now returns an instance of `Illuminate\Support\HtmlString` when running inside Laravel.  
- All references to the `QrCode` facade should be updated to:

```php
use SimpleSoftwareIO\QrCode\Facades\QrCode;
````

---

<a id="docs-configuration"></a>

## Configuration

#### Composer

Install via Composer:

```bash
composer require devsappsltd/simple-qrcode "~4"
```

Laravel will automatically register the package.

#### Service Provider (Laravel <= 5.4)

Add this line to the `providers` array in `config/app.php`:

```php
SimpleSoftwareIO\QrCode\QrCodeServiceProvider::class,
```

#### Alias (Laravel <= 5.4)

Add this line to the `aliases` array in `config/app.php`:

```php
'QrCode' => SimpleSoftwareIO\QrCode\Facades\QrCode::class,
```

---

<a id="docs-ideas"></a>

## Simple Ideas

#### Print View

```blade
<div class="visible-print text-center">
    {!! QrCode::size(100)->generate(Request::url()); !!}
    <p>Scan me to return to the original page.</p>
</div>
```

#### Embed a QrCode in Email

```blade
<img src="{!! $message->embedData(QrCode::format('png')->generate('Embed me into an e-mail!'), 'QrCode.png', 'image/png') !!}">
```

---

<a id="docs-usage"></a>

## Usage

#### Basic Usage

```php
use SimpleSoftwareIO\QrCode\Facades\QrCode;

QrCode::generate('Make me into a QrCode!');
```

This will generate a QR code that says **"Make me into a QrCode!"**

#### Generate `(string $data, string $filename = null)`

```php
QrCode::generate('Make me into a QrCode!', '../public/qrcodes/qrcode.svg');
```

#### Format `(string $format)`

```php
QrCode::format('png');
QrCode::format('eps');
QrCode::format('svg');
```

> `imagick` is required for PNG generation.

#### Size `(int $size)`

```php
QrCode::size(200);
```

#### Color `(int $r, int $g, int $b, int $alpha = null)`

```php
QrCode::color(255, 0, 0);       // Red
QrCode::color(255, 0, 0, 25);   // Red with 25% transparency
```

#### Background Color

```php
QrCode::backgroundColor(255, 255, 0); // Yellow background
```

#### Gradient

```php
QrCode::gradient(255, 0, 0, 0, 0, 255, 'diagonal');
```

Supports: `vertical`, `horizontal`, `diagonal`, `inverse_diagonal`, `radial`.

#### Eye Color

```php
QrCode::eyeColor(0, 255, 255, 255, 0, 0, 0);
```

#### Style

```php
QrCode::style('dot');
```

Supports: `square`, `dot`, `round`.

#### Eye Style

```php
QrCode::eye('circle');
```

Supports: `square`, `circle`.

#### Margin

```php
QrCode::margin(50);
```

#### Error Correction

```php
QrCode::errorCorrection('H');
```

Levels: **L (7%)**, **M (15%)**, **Q (25%)**, **H (30%)**

#### Encoding

```php
QrCode::encoding('UTF-8')->generate('Symbols ♠♥!!');
```

#### Merge Image

```php
QrCode::format('png')
    ->merge('path-to-logo.png', 0.3)
    ->errorCorrection('H')
    ->generate();
```

#### Merge String

```php
QrCode::format('png')
    ->mergeString(Storage::get('path/to/image.png'), 0.3)
    ->generate();
```

---

<a id="docs-helpers"></a>

## Helpers

#### Email

```php
QrCode::email('foo@bar.com', 'Subject', 'Message body');
```

#### Geo Location

```php
QrCode::geo(37.822214, -122.481769);
```

#### Phone Number

```php
QrCode::phoneNumber('555-555-5555');
```

#### SMS

```php
QrCode::SMS('555-555-5555', 'Message text');
```

#### WiFi

```php
QrCode::wiFi([
    'ssid' => 'NetworkName',
    'encryption' => 'WPA',
    'password' => 'secret'
]);
```

---

<a id="docs-common-usage"></a>

## Common Use Cases

| Usage           | Prefix       | Example                                                          |
| --------------- | ------------ | ---------------------------------------------------------------- |
| Website URL     | http\://     | [http://example.com](http://example.com)                         |
| Secure URL      | https\://    | [https://example.com](https://example.com)                       |
| Email Address   | mailto:      | mailto\:hello\@example.com                                       |
| Phone Number    | tel:         | tel:555-555-5555                                                 |
| SMS             | sms:         | sms:555-555-5555\:Hello                                          |
| Geo Coordinates | geo:         | geo:37.822214,-122.481769                                        |
| MeCard          | mecard:      | MECARD\:N\:Doe, John;TEL:555-555-5555;EMAIL\:hello\@example.com; |
| vCard           | BEGIN\:VCARD | [Wikipedia](https://en.wikipedia.org/wiki/VCard)                 |
| WiFi            | wifi:        | wifi\:WPA;NetworkName;Password;Hidden(false)                     |

---

<a id="docs-outside-laravel"></a>

## Usage Outside Laravel

```php
use SimpleSoftwareIO\QrCode\BaconQrCodeGenerator;

$qrcode = new BaconQrCodeGenerator;
$qrcode->size(500)->generate('Make a QrCode without Laravel!');
```

---

## License

This software is released under the [MIT license](LICENSE).
Copyright © 2014–2020 Simple Software LLC
Copyright © 2025 DevsApps LTD

```
