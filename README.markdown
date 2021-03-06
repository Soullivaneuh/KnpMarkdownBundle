Provide markdown conversion (based on [Michel Fortin work](https://github.com/michelf/php-markdown)) to your Symfony2 projects.

[![Build Status](https://secure.travis-ci.org/KnpLabs/KnpMarkdownBundle.png)](http://travis-ci.org/KnpLabs/KnpMarkdownBundle)

## INSTALLATION

Add KnpMarkdownBundle to your `composer.json`

```yaml
{
    "require": {
        "knplabs/knp-markdown-bundle": "~1.3"
    }
}
```
or simply run ```composer require knplabs/knp-markdown-bundle:~1.3```

Register the bundle in ``app/AppKernel.php``

```php
$bundles = array(
    // ...
    new Knp\Bundle\MarkdownBundle\KnpMarkdownBundle(),
);
```

## USAGE

```php
// Use the service
$html = $this->container->get('markdown.parser')->transformMarkdown($text);

// Use the helper with default parser
echo $view['markdown']->transform($text);

// Use the helper and a select specific parser
echo $view['markdown']->transform($text, $parserName);
```

If you have enabled the Twig markdown filter, you can use the following in your Twig templates:

```twig
{# Use default parser #}
{{ my_data|markdown }}

{# Or select specific parser #}
{{ my_data|markdown('parserName') }}
```

## Change the parser implementation

Create a service implementing `Knp\Bundle\MarkdownBundle\MarkdownParserInterface`,
then configure the bundle to use it:

```yaml
knp_markdown:
    parser:
        service: my.markdown.parser
```

Alternatively if you are using the ``markdown.parser.sundown`` there are
options for enabling sundown extensions and render flags, see the
default Configuration with:

    php app/console config:dump-reference knp_markdown

This bundle comes with 5 parser services, 4 based on the same algorithm
but providing different levels of compliance to the markdown specification,
and one which is uses the php sundown extension:

    - markdown.parser.max       // fully compliant = slower (default implementation)
    - markdown.parser.medium    // expensive and uncommon features dropped
    - markdown.parser.light     // expensive features dropped
    - markdown.parser.min       // most features dropped = faster
    - markdown.parser.sundown   // faster and fully compliant (recommended)

``markdown.parser.sundown`` requires the [php sundown extension](https://github.com/chobie/php-sundown).

For more details, see the implementations in Parser/Preset.

## TEST

    phpunit -c myapp vendor/bundles/Knp/Bundle/MarkdownBundle
