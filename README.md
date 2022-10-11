## Blade Icon Generation Helpers
A helper package to convert the icons to make it compatible to be used with the `blade-icons` package.

### Installation
```php
composer require codeat3/blade-icon-generation-helpers
```

### Usage
Kindly refer the [Generating Icons](https://github.com/blade-ui-kit/blade-icons#generating-icons) section from [Blade Icons](https://github.com/blade-ui-kit/blade-icons). This Package utilizes the `after` callback to process the icons as per our need.

The `after` callback received 3 arguments `string $icon` - this is the pathname of the icon, `array $config` - the config specified in the `generation.php` & `SplFileInfo $file` - the original icon file instance.

Inside a callback we instantiate our Processor class & use its helper methods to our benefit.

```php
$iconProcessor = new IconProcessor($icon, $config, $sourceFile);
```

This `IconProcessor` instance itself does some optimization on its own by default. Minimal usage would be as follows.
```php
$iconProcessor = new IconProcessor($icon, $config, $sourceFile);
    $iconProcessor
        ->optimize()
        ->save();
```

The `optimize` method does the following things by default:
* it removes the following attributes from the svg string
    - 'width', 'height', 'class', 'style', 'id',
* if in the config provided `is-outline` is set as `true`
    - if present - it sets the `fill` attribute as `none`
    - if present - it sets the `stroke` attribute as `currentColor`
* if in the config provided `is-solid` is set as `true`
    - if present - it sets the `fill` attribute as `currentColor`
* if in the config provided `custom-attributes` is set`
    - it will set the attributes to the svg string
* optionally it provides 2 callable methods `pre` & `post`
    - both callback receives the `$svgEL` as the `DOMDocument` - its the DOMDocument instance of the current icon
    - `pre` can be used if you have to perform some operation before Processor does the default processing
    - `post` can be used if you have to perform some operation after Processor does the default processing

The `save` method
